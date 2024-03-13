# calico使用手册

## calico
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