# 系统镜像

## 系统内核
- 编译内核: make defconfig; make -j4
- 系统内核: /boot/vmlinuz-xx

## diskimage-builder
```bash
DIB_RELEASE=jammy && disk-image-create -o demo.qcow2 apt-preferences vm block-device-efi ubuntu
```

## image使用
```bash
#镜像快照
qemu-img create -f qcow2 -b os.qcow2 snap.qcow2
#格式转换
qemu-img convert -f qcow2 -O raw demo.qcow2 demo.disk
#修改磁盘
qemu-img resize demo.disk +20G
```
