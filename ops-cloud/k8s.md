# K8s 使用

## k8s 部署
kubespray

## Helm 操作

## 常规命令
### 多个Pod执行命令
```bash
kubectl get pod |grep worker|grep -v demo|awk '{print $1}'|xargs -I {} kubectl exec -it {} -- nvidia-smi
```

### 暴露ClusterIp
```bash
kubectl -n demo expose svc mysql-default --port=3306 --target-port=3306 --name=mysql-demo
```

### Network
```bash
kubectl get cm -n kube-system kube-proxy -oyaml         #ipvs
kubectl get cm -n kube-system nodelocaldns -oyaml       #localdns
```