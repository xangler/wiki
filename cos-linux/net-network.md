# 网络相关操作

## 网络问题
```bash
# 硬件层排查
sar -n DEV 1 1 #网卡性能
netstat -i
# 链路层排查
arp -an
arping 10.0.0.2
# 网络层排查
route
ping www.baidu.com
traceroute www.baidu.com
# 传输层排查
netstat -s #协议总览
ss -anp|grep 80 #主机端口
netstat -anp|grep 80 #主机端口
telnet 127.0.0.1 9092 #远程端口
# 应用层排查
nslookup www.baidu.com
dig www.baidu.com ns +trace
mtr -z www.iq.com
# 业务层排查
iftop -P
```

## 网络服务
```bash
nc -l -p 80
nc -vzw1 127.0.0.1 80
python3 -m http.server 80
iperf3 -s -p 80
iperf3 -c 10.100.100.100 -p 30080
```

## 端口转发
```bash
socat TCP-LISTEN:30080,fork,reuseaddr  TCP:192.168.0.2:30080
ssh -f -N -L 127.0.0.1:30900:${target_ip}:30900 root@${jump_ip} -p 22
```

## 网络抓包
```bash
tcpdump -pne -i eth0
tcpdump tcp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
tcpdump udp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
```

## HTTP 性能测试
```bash
# time_namelookup : 从请求开始到DNS解析完成的耗时
# time_connect : 从请求开始到TCP三次握手完成耗时
# time_appconnect : 从请求开始到TLS握手完成的耗时
# time_pretransfer : 从请求开始到向服务器发送第一个GET请求开始之前的耗时
# time_redirect : 重定向时间，包括到内容传输前的重定向的DNS解析、TCP连接、内容传输等时间
# time_starttransfer : 从请求开始到内容传输前的时间
# time_total : 从请求开始到完成的总耗时
curl -w '\ntime_namelookup=%{time_namelookup}\ntime_connect=%{time_connect}\ntime_appconnect=%{time_appconnect}\ntime_redirect=%{time_redirect}\ntime_pretransfer=%{time_pretransfer}\ntime_starttransfer=%{time_starttransfer}\ntime_total=%{time_total}\n\n' -o /dev/null -s -L 'https://www.thebyte.com.cn/'
```
