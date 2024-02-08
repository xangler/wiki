# 磁盘基本操作

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
(parted) mkpart primary ext4 1499500 1998900 #ext2/linux-swap
# (parted) toggle 1 lvm  #设置为lvm
(parted) quit

#swap分区格式化
mkswap /dev/sda3

#新增分区格式化
mkfs.xfs /dev/sdf2

#物理分区扩容
resize2fs /dev/sda2

#自动挂载与卸载
mkdir -p /mnt/xdisk
#ls -l /dev/disk/by-uuid/ |grep md0 
#新增/etc/fstab配置
#UUID=a5c4f829-227a-4c10-82af-d9b1b7999a65 /mnt/xdisk   xfs    defaults,noatime    0   0
#手动挂载
mount -a

#手动卸载
umount /dev/md0 /mnt/xdisk
#删除/etc/fstab配置
```

## 文件系统操作
```bash
df -lh #查看硬盘使用情况
du -lh -d 1 #查看文件夹大小
dd if=/dev/zero of=x1 bs=1M count=1000
```

## 进程压力
```bash
iostat -x 1
iotop -P
```

## 硬盘性能压测
|           |IOPS               |延时                       |吞吐量              |  
|-          |-                  |-                          |-                  |
|read       |                   |                           |Read_PPS_Testing   |
|write      |                   |                           |Write_PPS_Testing  |  
|randread   |Rand_Read_Testing  |Rand_Read_Latency_Testing  |                   |  
|randwrite  |Rand_Write_Testing |Rand_Write_Latency_Testing |                   |

```bash
fio -direct=1 -iodepth=128 -rw=randread -ioengine=libaio -bs=4k -size=1G -numjobs=1 -runtime=1000 -group_reporting -filename=/dev/your_device -name=Rand_Read_Testing
```