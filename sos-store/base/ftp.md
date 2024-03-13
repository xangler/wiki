# ftp 使用文档

## ftp 服务管理
- systemctl status vsftpd
- 配置文件: /etc/vsftpd/vsftpd.conf
- 数据路径: /var/ftp

## ftp 常规配置
```bash
#匿名用户
# 允许匿名用户作为user(ftp)下载数据路径文件
# anonymous_enable=YES
#本地用户(可读/目录，只能写${HOME}目录)
local_enable=YES
write_enable=YES
local_umask=022
#黑白名单
# 是否开启黑白名单
# userlist_enable=YES
# 配合/etc/vsftpd/user_list作为白名单
# userlist_deny=NO
```

## ftp虚拟用户
- 创建系统用户
    - useradd -d /mnt/ftp fvuser -s /sbin/nologin
- 创建虚拟用户
    - 配置/etc/vsftpd/vuserlist(奇数行为用户名，偶数行为用户密码)
    - db_load -T -t hash -f /etc/vsftpd/vuserlist /etc/vsftpd/vuserlist.db
    - chmod 600 /etc/vsftpd/vuserlist.db
    - 配置/etc/pam.d/fvd 新增如下记录
```bash
auth required pam_userdb.so db=/etc/vsftpd/vuserlist
account required pam_userdb.so db=/etc/vsftpd/vuserlist
```
- 创建虚拟用户目录
    - mkdir -p /mnt/ftp/fdemo1/rootdir
    - chown -R fvuser.fvuser /mnt/ftp/fdemo1
    - chmod 500 /mnt/ftp/fdemo1
- 修改ftp配置
```bash
#pam文件名
pam_service_name=fvd
#开启虚拟用户
guest_enable=YES
#指定虚拟用户的宿主用户
guest_username=fvuser
#虚拟用户使用本地用户的权限
virtual_use_local_privs=YES
#所有用户启用chroot隔离
chroot_local_user=YES
#替换的标记
user_sub_token=$USER
#用户登录后的根目录
local_root=/mnt/ftp/$USER
```

## 客户端使用
- lftp 172.0.0.1