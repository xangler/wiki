# ntp服务文档

## 查看物理机时区
- timedatectl
- Local Time: 本地时间
- Universal Time: 世界时间
- RTC Time: 主板时间
 
## 时间同步
- systemctl status chronyd
- 配置文件: /etc/chrony.conf
- 同步状态: chronyc sources -v
- 服务器配置:
    - allow 10.1.0.0/24
    - local stratum 10
- 客户端配置:
    - server 10.1.0.2