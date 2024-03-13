# dhcp 使用

## dhcp 管理
- systemctl status dhcpd
- 配置文件: /etc/dhcp/dhcpd.conf

## dhcp 配置
```bash
#指定 DNS 服务器的 IP 地址
option domain-name-servers 8.8.8.8, 8.8.4.4;
#指定分配的 IP 地址的默认租约时间
default-lease-time 600;
#指定分配的 IP 地址的最大租约时间
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option subnet-mask 255.255.255.0;
    option routers 192.168.1.1;
    #指定 PXE 客户端应该从哪个服务器下载引导文件;
    #next-server 192.168.1.10;
    #filename "pxelinux.0";
}
```