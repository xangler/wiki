# 账号操作

## 创建账号并设定密码
```bash
useradd demo
passwd demo
```

## 添加sudo权限
```bash
chmod -v u+w /etc/sudoers
vi /etc/sudoers

## Allow root to run any commands anywher
root ALL=(ALL) ALL
demo ALL=(ALL) NOPASSWD:ALL #新增用户信息
 
chmod -v u-w /etc/sudoers
```

## 添加docker权限
```bash
usermod -aG root demo
```

## 普通账号开启ssh权限
```bash
vi /etc/ssh/sshd_config
#注释掉 AllowUsers root
service sshd restart
```