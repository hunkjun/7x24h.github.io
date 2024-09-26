---
title: Linux网络问题排查及信息采集
date: 2024-06-19 11:02:06
tags: 脚本
---

在排查网络问题时，有时会遇到 TCP ping 丢包的情况，但这种情况可能是偶发的。当我们准备进行排查时，丢包现象可能又消失了。为了持续采集信息并更好地诊断问题，可以使用以下脚本进行实时数据获取，以便及时发现和解决问题。

### ping 信息获取

- ping值带时间戳

```shell
#!/bin/bash
# 10分钟一采集数据
while true; do 
    # 定义变量
    ips=(xx.xx.xx.172 xx.xx.xx.59 xx.xx.xx.71)
    dt=`date +"%Y%m%d-%H%M%S"`
    # 后台执行
    for ip in ${ips[@]}; do 
        logfile=ping_${ip}_${dt}.log
        date -u >> $logfile
        nohup sudo /bin/ping -i 0.1 -c 6000 $ip | awk '{ print $0"\t" strftime("%Y-%m-%d %H:%M:%S",systime()); fflush()}'  >> $logfile &
        date -u  >> $logfile
    done
    # sleep 600
    sleep 600
    # 清理
    ls -rht ping*log |head -n -400| xargs rm
done

```

### mtr 信息获取

运行下面的shell脚本

```shell

#!/bin/bash

while true; do 
    targets=(47.xx.xx.59 47.xx.xx.71)
    tcp_port=20131
    outfile=mtr-result-$(date -u +'%Y%m%d%H%M').txt

    log() {
        msg=$1
        echo "$(date -u +'%Y-%m-%d %H:%M:%S'): ${msg}" >> ${outfile}
    }

    for dst in ${targets[@]}
    do 
    log ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    log "diagnostic network using mtr with icmp, target: ${dst}: started."
    mtr -r -c1 ${dst} >> ${outfile}
    log "diagnostic using mtr with icmp, target: ${dst}: done."
    done


    for dst in ${targets[@]}
    do 
    log ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    log "diagnostic network using mtr with tcp, target: ${dst}, port: ${tcp_port}: started."
    mtr -r --tcp --port ${tcp_port} -c1 ${dst} >> ${outfile}
    log "diagnostic using mtr with tcp, target: ${dst}, port: ${tcp_port}: done."
    done

    sleep 1
done
```

### tcpdump 抓包

- 示例1

```shell

#!/bin/bash

while true; do
   tcpdump -i any port 8080 -s 0 -G 600 -W 6 -w /data/tcpdumpv1/capture-`hostname -s`-%Y%m%d-%H%M%S.pcap
   ls -rht capture*pcap|head -n -400| xargs rm
done

```

- 示例2

```shell
#!/bin/bash

while true; do
   tcpdump -i eth0 host 10.xx.0.62 and \(host 11.xx.xx.33 \) -s 0 -G 600 -W 6 -w /data/tcpdumpv1/capture-`hostname -s`-%Y%m%d-%H%M%S.pcap
   ls -rht capture*pcap|head -n -400| xargs rm
done
```