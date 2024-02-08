# 磁盘

## 本地磁盘
HDD: 盘面 -> 磁道 -> 扇区
- ATA指令:IDE(并) SATA(串)
- SCSI指令:SCSI(并) SAS(串)
SSD:
- LUN -> Plane -> Block(擦除) -> Page(读写) -> Cell
- FTL(LBA -> PBA)

## 网络存储
- DAS: SCSI + 直连
- SAN: ISCSI + FC/网络 --- 块存储
- NAS: CIFS/NFS + 网络 --- 文件存储

## RDMA
- IB
- RoCE
- iWARP

## 两地三中心
- 主从备份
- 服务双活