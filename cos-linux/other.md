# 其它常用操作

<div align=center><img src="linux.drawio.svg"/></div>  

## 系统日志
```bash
cat /var/log/messages   #系统日志
```

## ansible多个节点执行命令
```bash
ansible all -m shell -a "df -lh|grep mysql"
```

## 多个Pod执行命令
```bash
kubectl get pod |grep worker|grep -v demo|awk '{print $1}'|xargs -I {} kubectl exec -it {} -- nvidia-smi
```

## 暴露ClusterIp
```bash
kubectl -n demo expose svc mysql-default --port=3306 --target-port=3306 --name=mysql-demo
```