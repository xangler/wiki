# cloudinit 使用

## cloudinit 管理
- systemctl status cloud-init.service
- 默认配置: /etc/cloud/cloud.cfg
- 执行记录: /var/lib/cloud/instances/${id}
- 执行日志: /run/cloud-init/

## cloudinit 配置
meta-data
```
```
user-data
```
```

## cloudinit iso
```bash
genisoimage -output cidata.iso -volid cidata -joliet -rock user-data meta-data
```

