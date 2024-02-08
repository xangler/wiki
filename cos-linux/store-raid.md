# 软raid搭建

## raid分类
- raid0: 磁盘容量相加
- raid1: 两两互备
- raid5: 一个校验盘
- raid6: 两个校验盘
- raid10: 先两两互备， 然后在直接相加组合

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

## 格式化磁盘
```bash
mkfs.xfs /dev/md0 # 格式化磁盘
```

## 磁盘挂载与卸载
```bash
mkdir -p /mnt/xdisk
#新增/etc/fstab配置
#/dev/md0 /mnt/xdisk   xfs    defaults,noatime    0   0
mount -a #手动挂载

umount /mnt/xdisk #手动卸载
#删除/etc/fstab配置
```

## 停止raid设备
```bash
mdadm -Ss /dev/md0
mdadm --zero-superblock /dev/sdb
#/etc/mdadm.conf 中删除对应记录
```
