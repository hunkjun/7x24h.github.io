---
title: 自建去除水印工具IOPaint
date: 2025-02-10 16:07:06
tags: IOPaint
---

## 遇到的问题

有一百万的图片要去除水印.

水印位置不固定，水印类型一致.

## IOPaint 工具研究

### docker 启动

```shell
docker run -p 8080:8080 \
-v /data/tmp-hcj/torch_cache:/root/.cache/torch \
-v /data/tmp-hcj/huggingface_cache:/root/.cache/huggingface \
--privileged=true \
--rm thr3a/iopaint start --model=lama --device=cpu --port=8080 --host=0.0.0.0
```

### 进入docker执行转化

```shell
docker exec -it 容器id bash

iopaint run --model=lama --device=cpu \
--image=./input \
--mask=./mask/test3_mask.jpg \
--output=./output
```

其中mask掩码图片需要到 127.0.0.1:8080 webui 页面提取.

## 限制

多张图片的掩码图片必须一致.

- https://github.com/Sanster/IOPaint/tree/main

- https://www.iopaint.com/batch_process