# 网络相关操作

## 探测服务端口
```bash
# 主机网络
netstat -anlp|grep 80
# 远程网络
nc -vzw1 127.0.0.1 9092
telnet 127.0.0.1 9092
```

## 端口转发
```bash
ssh -f -N -L 127.0.0.1:30900:${target_ip}:30900 root@${jump_ip} -p 22
```

## 防火墙的使用
```bash
firewall-cmd --list-all
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --reload
```

## 视频流抓包
```bash
tcpdump tcp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
tcpdump udp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
```