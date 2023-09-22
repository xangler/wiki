# 软raid搭建

## 组建软raid
```bash
# 查看raid 磁盘信息
mdadm -Ds
# 使用/dev/sdb /dev/sdc  2块磁盘组建 /dev/md0 一个raid0 磁盘
mdadm -C -v /dev/md0 -l 0 -n 2 /dev/sdb /dev/sdc
# 使用/dev/sdd /dev/sde  2块磁盘 + /dev/sdf 1块热备份磁盘 组建 /dev/md1 一个raid1 磁盘
mdadm -C -v /dev/md1 -l 1 -n 2 -x 1 /dev/sd[d,e,f]  && mdadm -Dsv > /etc/mdadm.conf
# 使用/dev/sdg /dev/sdh /dev/sdi  3块磁盘 + /dev/sdj 1块热备份磁盘 组建 /dev/md5 一个raid5 磁盘
mdadm -C -v /dev/md5 -l 5 -n 3 -x 1 -c 32 /dev/sd{g,h,i,j} && mdadm -Dsv > /etc/mdadm.conf
```

## 格式化磁盘与挂载
```bash
# 格式化磁盘
mkfs.xfs /dev/md0
# 挂载磁盘
mkdir raid_0 && mount /dev/md0 /dev/raid_0/
```

## 开机自动挂载
```bash
# 在/etc/fstab文件中添加
/dev/md0        /dev/raid_0     xfs     defaults        0 0
```

# 软raid删除
## 清除开机挂载
```bash
# /etc/fstab 中删除对应记录
```

## 取消挂载
```bash
# umount /dev/md0 /dev/raid_0
```

## 停止raid设备
```bash
mdadm -Ss /dev/md0
mdadm --zero-superblock /dev/sdb
#/etc/mdadm.conf 中删除对应记录
```
