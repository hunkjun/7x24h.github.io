---
title: 常用shell脚本
date: 2023-12-15 18:09:58
tags:
---
## 日常脚本

- 定时监控url访问状态，不为200时进行dingding告警

```

## 脚本文件: curl_monitor.sh
## 脚本内容:

#!/bin/bash

url=https://baidu.com

function send_msg() {
	msg="$1 ,msgFrom `hostname`"
	curl 'https://oapi.dingtalk.com/robot/send?access_token=f4d9ac1b17368f1727e575b8fac1ee8aca2b7c6316300477fe18f861330bab2b' \
	-H 'Content-Type: application/json' \
	-d "{'msgtype': 'text',
	  'text': {
	       'content': \"$msg\"
	  }
    }"
}

function curl_monitor() {
	http_code=`curl -I -m 10 -o /dev/null -s -w %{http_code} $url`
	if [ $http_code -eq 200 ]; then
	   echo "`date` Request $url successful." >> curl.log
	else
	   echo "`date` Request $url failed with response code: $http_code" >> curl.log
	   send_msg "`date` Request $url failed with response code: $http_code"
	fi
}

curl_monitor;

## 定时任务

10 * * * * /bin/bash /data/scripts/curl_monitor.sh

```