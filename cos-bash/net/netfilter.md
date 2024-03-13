# netfilter相关操作

## 网络查看
- ethtool: 网卡配置与查看
- ifconfig: ip配置与查看
- route: 路由配置与查看
- iw: 无线网络配置与查看
- ip: 通用网络配置与查看
- nmcli: 简易网络配置与查看

## 网络默认配置
- 默认配置: /etc/networks
- 网卡启动配置: /etc/sysconfig/network-scripts/ifcfg*

## 域名解析配置
- 本地host: /etc/hosts
- dns服务: /etc/resolv.conf
- 先后顺序: /etc/nsswitch.conf

## ipair
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

## ibridge
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

## nftables
```bash
# 创建
nft add table inet xtable
nft add chain inet xtable input { type filter hook input priority 0\; }
nft add rule inet xtable input tcp dport ssh accept
# 查看
nft --handle list chain inet xtable input
#销毁
nft delete rule inet xtable input handle 1
nft delete chain inet xtable input
nft delete table inet xtable
```

## iptables
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
iptables -t nat -A KUBE-POSTROUTING -j MASQUERADE --random-fully
```

## traffic control
```bash
```

