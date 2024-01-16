# Syscall

## namespace 操作
```bash
nsenter -n -t ${PID}
```

## cgroup 操作
```bash
# /sys/fs/cgroup/cpu
```

## 压测
```bash
stress --cpu 1 --timeout 60
stress --vm-bytes 100m --vm-keep -m 1
```

## ipc 操作 
```bash
ipcmk -Q
ipcs
```

## strace 操作
```bash
strace ./main.out
```