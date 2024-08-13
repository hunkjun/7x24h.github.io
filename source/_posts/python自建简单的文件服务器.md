---
title: python自建简单的文件服务器
date: 2024-08-13 17:54:36
tags:
---

### 为啥有这个?

内网主机之间无法通过scp拷贝文件。通过如下脚本启动pytho程序接收文件。

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
