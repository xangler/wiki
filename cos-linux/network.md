# 网络相关操作

## ARP
```bash
arp -an
arping 10.0.0.2
```

## 网络配置
```bash
ifconfig
```

## VPair
```bash
# 创建netns
ip netns add ns1
ip netns add ns2
# 创建peer
ip link add veth-eth0 type veth peer name eth0-veth
ip link set dev eth0-veth netns ns1
ip link set dev veth-eth0 netns ns2
# 网络配置
ip netns exec ns1 ip link set dev eth0-veth up
ip netns exec ns1 ip addr add 10.0.0.2/24 dev eth0-veth
ip netns exec ns2 ip link set dev veth-eth0 up
ip netns exec ns2 ip addr add 10.0.0.3/24 dev veth-eth0
# 网络测试
ip netns exec ns2 ping -c2 10.0.0.2
# 查找对端
# ethtool -S eth0 # peer_ifindex
```

## VBridge
```bash
# 创建bridge
ip link add br0 type bridge
ip link set dev br0 up
ip addr add 10.0.0.1/24 dev br0
# 创建ns1
ip netns add ns1
ip link add ns1-eth0 type veth peer name eth0-ns1
ip link set dev ns1-eth0 up
ip link set dev ns1-eth0 master br0
ip link set dev eth0-ns1 netns ns1
ip netns exec ns1 ip link set dev eth0-ns1 up
ip netns exec ns1 ip addr add 10.0.0.2/24 dev eth0-ns1
ip netns exec ns1 ip route add default via 10.0.0.1 dev eth0-ns1
# 测试网络
ip netns exec ns1 ping -c2 10.0.0.1
# 创建ns2
ip netns add ns2
ip link add ns2-eth0 type veth peer name eth0-ns2
ip link set dev ns2-eth0 up
ip link set dev ns2-eth0 master br0
ip link set dev eth0-ns2 netns ns2
ip netns exec ns2 ip link set dev eth0-ns2 up
ip netns exec ns2 ip addr add 10.0.0.3/24 dev eth0-ns2
ip netns exec ns2 ip route add default via 10.0.0.1 dev eth0-ns2
# 测试网络
ip netns exec ns2 ping -c2 10.0.0.1
# 修改iptables
iptables -t filter -A FORWARD --in-interface br0 --jump ACCEPT
# 测试网络
ip netns exec ns2 ping -c2 10.0.0.2
```

## Calico
```bash
# IPIP
## 创建ns1
ip netns add ns1
ip link add ns1-eth0 type veth peer name eth0-ns1
ip link set dev eth0-ns1 netns ns1
ip netns exec ns1 ip link set dev eth0-ns1 up
ip netns exec ns1 ip addr add 10.0.0.2/24 dev eth0-ns1
ip netns exec ns1 ip route add 169.254.1.1 dev eth0-ns1 scope link
ip netns exec ns1 ip route add default via 169.254.1.1 dev eth0-ns1
## 创建ns2
ip netns add ns2
ip link add ns2-eth0 type veth peer name eth0-ns2
ip link set dev eth0-ns2 netns ns2
ip netns exec ns2 ip link set dev eth0-ns2 up
ip netns exec ns2 ip addr add 10.0.0.3/24 dev eth0-ns2
ip netns exec ns2 ip route add 169.254.1.1 dev eth0-ns2 scope link
ip netns exec ns2 ip route add default via 169.254.1.1 dev eth0-ns2
## 主机
ip link set dev ns1-eth0 up
ip link set dev ns2-eth0 up
echo 1 > /proc/sys/net/ipv4/conf/ns1-eth0/proxy_arp
echo 1 > /proc/sys/net/ipv4/conf/ns2-eth0/proxy_arp
ip route add 10.0.0.2/24 dev ns1-eth0 scope link
ip route add 10.0.0.3/24 dev ns2-eth0 scope link
## 跨机
# ip route add 10.0.1.2/24 via 172.16.0.1 dev ens192
```

## Iptables
```bash
# iptables -t nat -nvL PREROUTING
# host curl pod
iptables -t nat --new KUBE-SERVICES
iptables -t nat --new KUBE-SVC-HTTP
iptables -t nat --new KUBE-SEP-HTTP1
iptables -t nat --new KUBE-SEP-HTTP2
iptables -t nat -A PREROUTING -j KUBE-SERVICES
iptables -t nat -A KUBE-SERVICES --destination 10.100.100.100 --protocol tcp --match tcp --dport 30080 -j KUBE-SVC-HTTP
iptables -t nat -A KUBE-SVC-HTTP --match statistic --mode random --probability 0.5 -j KUBE-SEP-HTTP1
iptables -t nat -A KUBE-SVC-HTTP -j KUBE-SEP-HTTP2
iptables -t nat -A KUBE-SEP-HTTP1 --protocol tcp --match tcp -j DNAT --to-destination 10.0.0.2:80
iptables -t nat -A KUBE-SEP-HTTP2 --protocol tcp --match tcp -j DNAT --to-destination 10.0.0.3:80
iptables -t nat -A OUTPUT -j KUBE-SERVICES
# pod curl pod
iptables -t nat --new KUBE-POSTROUTING
iptables -t nat -A POSTROUTING -j KUBE-POSTROUTING
iptables -t nat -A KUBE-POSTROUTING -m comment --comment "kubernets pod snat" -j MASQUERADE --random-fully
```

## IPVS
```bash
# lsmod|grep ip_vs
# ipvsadm -ln
ipvsadm -A -t 10.100.100.100:30080 -s rr
ipvsadm -a -t 10.100.100.100:30080 -r 10.0.0.2:80 -m -w 1
```

## 端口转发
```bash
socat TCP-LISTEN:30080,fork,reuseaddr  TCP:192.168.0.2:30080
ssh -f -N -L 127.0.0.1:30900:${target_ip}:30900 root@${jump_ip} -p 22
```

## 防火墙的使用
```bash
firewall-cmd --list-all
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --reload
```

## DNS
```bash
ping www.baidu.com
nslookup www.baidu.com
dig www.baidu.com ns +trace
traceroute www.baidu.com
```

## 探测服务端口
```bash
# 主机网络
netstat -anlp|grep 80
# 远程网络
nc -vzw1 127.0.0.1 9092
telnet 127.0.0.1 9092
```

## 网络性能压测
```bash
# python3 -m http.server 80
iperf3 -s -p 80
iperf3 -c 10.100.100.100 -p 30080
```

## 视频流抓包
```bash
tcpdump tcp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
tcpdump udp -i en0 -t -s 0 -c 2000 port 9051 and host 127.0.0.1 -w ./target.cap
# tcpdump -pne -i eth0
```
