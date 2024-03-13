# grub 使用

## 修复grub
```bash
# 硬盘引导恢复
grub2-install /dev/vda
# 文件引导恢复
grub2-mkconfig > /boot/grub2/grub.cfg
# grub > set root='hd0,msdos1'
# grub > linux16 /vmlinuz-xxx ro root=/dev/vda1
# grub > initrd16 /initramfs-xx.img
# grub > boot
```

## 修复vmlinuz丢失
```bash
cp iso/Packages/kernel-xxx.rpm /mnt
rpm2cpio kernel-xxx.rpm | cpio -id #boot etc lib
```

## 修复initramfs丢失
```bash
mkinitrd /boot/initramfs-$(uname -r).img $(uname -r)
```