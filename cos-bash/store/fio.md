# fio使用文档

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