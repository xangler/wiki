# virsh 使用教程

## virsh 管理
- 配置文件: /etc/libvirt/qemu/*.xml
- 镜像文件: /var/lib/libvirt/images/*.qcow2

## vm 管理
```bash
virsh define vm.xml #配置虚拟机
virsh list --all    #查看虚拟机
virsh start vm      #启动虚拟机
virsh shutdown vm   #软关机
virsh destroy vm    #硬关机
virsh undefine vm   #删除配置, 不会删除对应qcow2
```

## 安装虚拟机
```bash
virt-install --cdrom rhel-dvd.iso \
--vcpus 1 --memory 1024 \
--disk demo.qcow2,size=8,bus=virtio \
--network bridge=br0,model=virtio \
--name demo
```
