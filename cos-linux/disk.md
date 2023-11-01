# 内存磁盘基本操作
## 内存操作
```bash
free -m #查看内存使用情况
swapon -s #查看是否有swap分区
swapoff -a #关闭swap分区
swapon /dev/sda3 #打开swap分区
```
## 硬盘操作
```bash
fdisk -l #查看硬盘信息
lsblk #查看硬盘信息

#增删分区
parted /dev/sdf
(oarted) mklabel gpt                   #有时候会用到
(parted) unit mb
(parted) p                    
(parted) rm 1                           #记得清除自动挂载
(parted) unit s
(parted) p
(parted) mkpart primary 1499500 1998900 #ext2/ext4/linux-swap

(parted) quit

#物理分区扩容
resize2fs /dev/sda2

#swap分区格式化
mkswap /dev/sda3

#新增分区格式化
mkfs.xfs /dev/sdf2

#自动挂载
ls -l /dev/disk/by-uuid/ |grep sdf2 
mkdir -p /mnt/locals/mysql/volume0
#vi /etc/fstab
#UUID=a5c4f829-227a-4c10-82af-d9b1b7999a65 /mnt/locals/mysql/volume0   xfs    defaults,noatime    0   0
#手动挂载
mount -a
```
## 文件系统操作
```bash
df -lh #查看硬盘使用情况
du -lh -d 1 #查看文件夹大小
```