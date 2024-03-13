# 账号操作

## 账号管理文件
- 账号默认配置: /etc/login.defs
- 账号文件: /etc/passwd
- 组文件: /etc/group
- 密码文件: /etc/shadows
    - 生成密码(whois): mkpasswd -m sha-512 demo
- 用户目录: /home/xxx
- 用户模板: /etc/skel/

## 账号管理
```bash
useradd demo
usermod -aG docker demo #用户新增组
usermod -G "" demo #清空用户组
usermod -L demo #冻结用户
usermod -U demo #解锁用户
userdel demo #删除用户, 不删除home目录
userdel -r demo #删除用户, 删除home目录
su - root #切换用户并修改工作路径
who -i #登陆信息
id #当前用户信息
```

## 账号sudo
- visudo
- 配置文件: /etc/sudoers
- demo ALL=(ALL) NOPASSWD:ALL #新用户配置

## 账号密码
```bash
echo demo |passwd --stdin demo
```
