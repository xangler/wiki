# K8s 使用

## registry 搭建
```bash
docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  -v /mnt/work/registry:/var/lib/registry \
  registry
```

## k8s 部署

## k8s 配置
### Network
```bash
kubectl get cm -n kube-system kube-proxy -oyaml         #ipvs
kubectl get cm -n kube-system nodelocaldns -oyaml       #localdns
```

## 常规命令
### 多pod日志
```bash
stern app -n ns
```

### 多个Pod执行命令
```bash
kubectl get pod |grep worker|grep -v demo|awk '{print $1}'|xargs -I {} kubectl exec -it {} -- nvidia-smi
```

### Helm 操作