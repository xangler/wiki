# 其它常用操作

## 日志采集
- systemctl status rsyslog
- 配置文件(类型+等级): /etc/rsyslog.conf

## 远程采集
- 客户端: 
    - *.* @10.1.0.2
- 服务端: 
    - $ModLoad imudp
    - $UDPServerRun 514
    - $template LOG, "%timegenerated% %fromhost-ip% %syslogfacility% %syslogtag%: %msg%\n"
    - *.* /var/log/demo;LOG

## 日志路径
- 系统日志: /var/log/messages
- 安全日志: /var/log/secure
- 定时任务: /var/log/cron
- local日志

## 内存日志
- 查看内存日志: journalctl
- 内存日志保存
```bash
mkdir /var/log/journal
chgrp systemd-journald /var/log/journal
chmod g+s /var/log/journal
killall -1 systemd-journald
```
