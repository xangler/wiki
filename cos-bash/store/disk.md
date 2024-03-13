# 磁盘基本操作

## 硬盘类型
- MBR: 首扇区记录信息，最多4个主分区
- GPT: 无限制

## 硬盘操作
- lsblk: 查看硬盘信息
- fdisk:磁盘分区与查看
    - fdisk /dev/sda
    - g: (*)设置磁盘类型
    - p/d/n: 打印/删除/创建分区
    - t: 设置分区类型
- parted:磁盘分区与查看
    - parted /dev/sda
    - mklabel gpt: (*)设置磁盘类型
    - p/rm/mkpart: 打印/删除/创建分区
    - toggle: 设置分区类型
- 磁盘格式化:
    - mkfs.xfs /dev/sda1: xfs分区格式化
- 磁盘扩缩容:
    - e2fsck -f /dev/sda1
    - resize2fs /dev/sda1
- 磁盘自动挂载:
    - mkdir -p /mnt/xdisk
    - blkid |grep sda1 
    - /etc/fstab配置
        - UUID=a5c4f829-227a-4c10-82af-d9b1b7999a65 /mnt/xdisk   xfs    defaults,noatime    0   0
    - mount -a
- 磁盘手动卸载(二选一即可):
    - umount /dev/sda1
    - umount /mnt/xdisk
- swap分区:
    - mkswap /dev/sda1: swap分区格式化
    - /etc/fstab配置
        - /dev/sda1   swap    swap    defaults    0   0
    - swapon -a: 手动挂载
    - swapoff -a: 手动关闭

## 硬盘限额
- 限额挂载:mount -o usrquota /dev/vdc1 demo
- 用户额度:edquota -u demo

## 硬盘加密(/dev/mapper/demo)
- 创建加密卷: cryptsetup luksFormat /dev/vdc1
- 打开加密卷: cryptsetup open /dev/vdc1 demo
- 格式化文件: mkfs.xfs /dev/mapper/demo
- 关闭加密卷: cryptsetup close demo
