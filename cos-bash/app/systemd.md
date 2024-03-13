# systemd 服务配置

## 基本操作
```bash
systemctl cat httpd     #查看服务配置
systemctl enable httpd  #允许开机启动
systemctl start httpd   #服务启动
systemctl status httpd  #服务状态
systemctl stop httpd    #服务停止
systemctl disable httpd #禁止开机启动
systemctl daemon-reload #重新加载配置
systemctl get-default   #查看开机启动项
systemctl set-default graphical.target  #设置图形启动
systemctl set-default multi-user.target #设置多用户启动
```

## 配置文件
- 开机启动服务: /etc/systemd/system
- 手动模式服务: /usr/lib/systemd/system

## 配置demo(sshd)
```
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
Type=simple
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

## 开机自启
- 配置文件: /etc/rc.d/rc.local

## 定时任务
- systemctl status crond
- 新增任务: crontab -e 
- 任务配置: /var/spool/cron/${USER}

## 单次任务
- 新增任务: at now+1h
- 任务列表: at -l
- 用户黑白名单: /etc/at.deny && /etc/at.allow

## bash默认执行
- .profile: 用户登录时加载该文件
- .bashrc: 用户新开shell时加载
