# Vnc 配置(rocky8.6)

## 安装桌面
```bash
# 安装Xfce
dnf --enablerepo=epel group -y install "Xfce" "base-x"
# 设置默认桌面
systemctl set-default graphical.target
```

## 安装Vnc-Server
```bash
dnf -y install tigervnc-server
```

## 配置防火墙
```bash
firewall-cmd --add-service=vnc-server
firewall-cmd --runtime-to-permanent
```

## 设置vnc密码
```bash
vncpasswd
```

## 配置vnc
vnc配置文件(${HOME}/.vnc/config)
```bash
session=gnome
securitytypes=vncauth,tlsvnc
geometry=1920x1080
```

vnc用户(/etc/tigervnc/vncserver.users)
```bash
# specify [:(display number)=(username] as comments
# display number 1 listens port 5901
# display number n + 5900 = listening port
:25001=demo
```

## 设置开机启动
```bash
systemctl enable --now vncserver@:25001
systemctl status  vncserver@:25001
```