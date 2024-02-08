# NFS 常用操作

## 配置nfs服务
```bash
# 创建共享目录
mkdir -p /nfs/data
## 配置共享目录
echo "/nfs/data/ *(insecure,rw,sync,no_root_squash)" >>/etc/exports
## 启动服务
systemctl enable rpcbind
systemctl enable nfs-server
systemctl start rpcbind
systemctl start nfs-server
## 启动nfs
exportfs -r
## 检查nfs
exportfs
```

## 挂载nfs路径
```bash
mount -t nfs 10.10.1.2:/nfs/data /mnt/nfs
```