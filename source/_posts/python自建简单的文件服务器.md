---
title: python自建简单的文件服务器
date: 2024-08-13 17:54:36
tags: 脚本
---

### 为啥有这个?

内网主机之间无法通过scp拷贝文件, 没有秘钥或没有密码。通过如下脚本启动pytho程序接收文件。

nc也可以，但是nc对于单文件来说比较好，多文件了就不行了。

### 服务器代码

```python
import os
from http.server import SimpleHTTPRequestHandler, HTTPServer

class CustomHTTPRequestHandler(SimpleHTTPRequestHandler):
    def do_PUT(self):
        length = int(self.headers['Content-Length'])
        path = self.translate_path(self.path)
        directory = os.path.dirname(path)

        # 创建目录结构
        if not os.path.exists(directory):
            os.makedirs(directory)

        with open(path, 'wb') as f:
            f.write(self.rfile.read(length))
        self.send_response(201, "Created")
        self.end_headers()

if __name__ == '__main__':
    server_address = ('', 8000)
    httpd = HTTPServer(server_address, CustomHTTPRequestHandler)
    print("Serving on port 8000...")
    httpd.serve_forever()
```

### 使用

#### 上传

```shell
curl -X PUT --upload-file 'xxx.log' http://xxx.96.xxx.60:8000/2024-08/
```

#### 下载

- 页面点击直接下载
- shell: wget http://localhost:8000/example.txt


---
上面的HTTPServer很好用，但是有个问题就是不支持多线程，导致文件较多的时候，效率就比较低. 下面将采取flask的方式进行，不好的一点就是要安装flask, 但支持多线程。
---


### server端

```python
import os
from flask import Flask, request, jsonify
from urllib.parse import unquote

app = Flask(__name__)

@app.route('/<path:filename>', methods=['PUT'])
def upload_file(filename):
    # 解码文件名，处理 URL 中的特殊字符
    decoded_filename = unquote(filename)

    # 确保请求中包含 Content-Length 头
    if 'Content-Length' not in request.headers:
        return jsonify({"error": "Content-Length header missing"}), 400

    # 获取文件的内容
    file_content = request.data

    # 获取文件路径，假设您希望将文件存储在 uploads 目录下
    file_path = os.path.join('uploads', decoded_filename)
    directory = os.path.dirname(file_path)
    print(directory)

    # 创建目录结构，确保所有父目录都存在
    if not os.path.exists(directory):
        try:
            os.makedirs(directory)
        except Exception as e:
            print(jsonify({"error": str(e)}))

    # 写入文件
    try:
        with open(file_path, 'wb') as f:
            f.write(file_content)
    except Exception as e:
        return jsonify({"error": str(e)}), 500

    return jsonify({"message": "File created", "path": file_path}), 201

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=9000, threaded=True)
```

### 客户端

```shell
# 可能文件名不支持#号，可能需要编码下
#  sed 's/#/%23/g'
curl -X PUT -T xxx.log http://10.40.129.133:9000/uploads/xxxxx.log
```

---
### 附

- 批量上传文件

```python
import os
import requests

# 定义目标 URL 的基本部分
base_url = 'http://10.96.104.60:9000/2024-09-02_07_bilog/'

# 读取文件路径列表
with open('file_paths.txt', 'r') as f:
    file_paths = f.readlines()

# 遍历文件路径并上传
for file_path in file_paths:
    file_path = file_path.strip()  # 去掉前后空白字符
    if not os.path.isfile(file_path):
        print(f"文件不存在: {file_path}")
        continue

    # 获取文件名
    file_name = os.path.basename(file_path)

    # 提取上传后端目录的最后两节
    subdir = os.path.dirname(file_path)
    path_parts = subdir.split(os.sep)
    if len(path_parts) >= 2:
        # 获取最后两部分
        last_two_parts = path_parts[-1:]  # 取最后两节
        print(last_two_parts)
        upload_path = os.path.join(*last_two_parts, file_name)  # 组合成上传路径
    else:
        print(f"路径不够深，无法构建上传路径: {subdir}")
        continue

    # 构建上传的 URL
    upload_url = os.path.join(base_url, upload_path)

    # 处理 URL 中的特殊字符（如 #）
    upload_url = upload_url.replace('#', '%23')

    # 打印上传信息
    print(f'Uploading {file_path} to {upload_url}')

    # 读取文件并进行 PUT 请求
    with open(file_path, 'rb') as file:
        response = requests.put(upload_url, data=file)

    # 输出响应信息
    print(f'Status Code: {response.status_code}, Response Body: {response.text}')
```

- 文件列表

```shell
cat file_paths.txt
/data/1com.log
/data/2com.log
/data/3com.log
```