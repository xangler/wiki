# firewall 使用

## firewall 服务管理
- systemctl status firewalld

## firewall 配置
```bash
firewall-cmd --list-all
firewall-cmd --add-service=vnc-server
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --add-masquerade --permanent #开启nat功能
firewall-cmd --runtime-to-permanent  #运行规则转为永久规则
firewall-cmd --reload
```