---
title: curl如何获取世界时间
date: 2023-12-20 13:40:02
tags: 脚本
---

有些主机的时间改了，但是需要和其他正常时间的主机对比一些日志，所以打日志的时候使用网络正确时间。

```shell
date -u -d @$(curl -s http://worldtimeapi.org/api/ip | jq -r '.unixtime') "+%Y-%m-%d %H:%M:%S"

2023-12-20 05:39:11
```