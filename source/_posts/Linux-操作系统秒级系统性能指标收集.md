---
title: Linux 操作系统秒级系统性能指标收集
date: 2023-12-29 09:30:21
tags: Linux，脚本
---

## 原因

普通的监控系统zabbix/prometheus一般数据采集间隔为30s或更长，有时候排查问题时间范围更小，比如db满日志2s/io突增，依据zabbix/prometheus的数据就很困难了。

## 脚本

每秒采集cpu/mem/io/net/mongostat的数据指标，轮询保留7天日志，每天的日志在200M的样子.

```shell
# 主要目标在排查数据库的慢日志、游戏服性能、UDP丢包等问题
#!/bin/bash

# 定义日志文件目录和文件名
LOG_DIR="/data/scripts/log"
LOG_FILE="sys_monitor_$(date +%Y%m%d).log"

# 确保日志目录存在
mkdir -p $LOG_DIR

# 定义统计的时间（秒）
MONITOR_TIME=150

# 定义统计的间隔（秒）
INTERVAL=2

# 后台运行 top 和 iostat 和 mongostat
# export COLUMNS=120
nohup /usr/bin/top -b -c -n $MONITOR_TIME -d $INTERVAL| grep -v ']'| grep -E ' ./|log-agent|load|Tasks|Cpu|Mem'| grep -v grep | gawk '{print strftime("%Y-%m-%d %H:%M:%S [top]: ") $0}' >> $LOG_DIR/$LOG_FILE &

nohup /usr/bin/iostat nvme1n1 nvme0n1 -x $INTERVAL $MONITOR_TIME | gawk '{print strftime("%Y-%m-%d %H:%M:%S [iostat]: ") $0}' >> $LOG_DIR/$LOG_FILE &

if [ -f "/usr/bin/mongostat" ]; then
    # echo "/usr/bin/mongostat --username=Username --password=Password --authenticationDatabase=admin --all -n $MONITOR_TIME $INTERVAL >> $LOG_DIR/$LOG_FILE "
    nohup /usr/bin/mongostat --username=Username --password=Password --authenticationDatabase=admin --all -n $MONITOR_TIME $INTERVAL | gawk '{print strftime("%Y-%m-%d %H:%M:%S [mongostat]: ") $0}'>> $LOG_DIR/$LOG_FILE &
fi

# 循环统计网络、磁盘、CPU、内存
for ((i=0; i<$MONITOR_TIME; i++)); do

    # 网络
    /sbin/ifconfig | gawk '{print strftime("%Y-%m-%d %H:%M:%S [ifconfig]: ") $0}' >> $LOG_DIR/$LOG_FILE
    /bin/netstat -s | xargs | gawk '{print strftime("%Y-%m-%d %H:%M:%S [netstat]:: ") $0}' >> $LOG_DIR/$LOG_FILE
    # /bin/netstat -i | gawk '{print strftime("%Y-%m-%d %H:%M:%S [netstat]: ") $0}' >> $LOG_DIR/$LOG_FILE
    #/sbin/ethtool -S eth0 | gawk '{print strftime("%Y-%m-%d %H:%M:%S [ethtool]: ") $0}' >> $LOG_DIR/$LOG_FILE
    #/sbin/ethtool -g eth0 | gawk '{print strftime("%Y-%m-%d %H:%M:%S [ethtool]: ") $0}' >> $LOG_DIR/$LOG_FILE

    # 内存
    /usr/bin/free -m | gawk '{print strftime("%Y-%m-%d %H:%M:%S [free]: ") $0}' >> $LOG_DIR/$LOG_FILE

    # CPU
    /usr/bin/mpstat | gawk '{print strftime("%Y-%m-%d %H:%M:%S [mpstat]: ") $0}' >> $LOG_DIR/$LOG_FILE

    # 等待下一次统计
    sleep $INTERVAL
done

# 删除7天前的旧文件
find $LOG_DIR -name "system_monitor_*.log" -mtime +5 -exec rm {} \;
```

## 分析

根据采集到的数据，截取时间和值，在excel里面进行绘图/折线图分析.