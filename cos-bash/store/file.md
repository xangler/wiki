# 文件操作

## 文件系统操作
```bash
df -lh #查看硬盘使用情况
du -lh -d 1 #查看文件夹大小
dd if=/dev/zero of=x1 bs=1M count=1000
```

## 文件权限
```bash
umask #创建文件/文件夹时权限
chattr +i file
chmod a+x file #o,u,g+t,x,s
chown demo.demo file
getfacl file
setfacl -m u:demo:rwx file
```

## 临时文件
```bash
#更新临时文件配置
systemd-tmpfiles --create /usr/lib/tmpfiles.d/*
#删除过期临时文件
systemd-tmpfiles --clean /usr/lib/tmpfiles.d/*
```

## 进程压力
```bash
iostat -x 1
iotop -P
```