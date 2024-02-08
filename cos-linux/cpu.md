# Syscall

## namespace 操作
```bash
nsenter -n -t ${PID}
```

## cgroup 操作
```bash
# /sys/fs/cgroup/cpu
```

## 进程状态
```bash
pidstate #-r 
```

## strace 操作
```bash
strace ./main.out
```

## 压测
```bash
stress --cpu 1 --timeout 60
stress --vm-bytes 100m --vm-keep -m 1
```
