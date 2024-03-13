# lvs使用手册

## ipvs
```bash
# ipvsadm -ln
ipvsadm -A -t 10.100.100.100:30080 -s rr
ipvsadm -a -t 10.100.100.100:30080 -r 10.0.0.2:80 -m -w 1
```