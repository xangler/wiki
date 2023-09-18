# Docker 配置

## 账号添加docker权限
```bash
sudo groupadd docker
sudo usermod -aG docker ${USER}
newgrp docker
```

## docker 配置
```bash
docker info
# /etc/docker/daemon.json
```

## docker pull 代理
修改配置(/etc/systemd/system/docker.service.d/http-proxy.conf)
```text
[Service]
Environment="HTTPS_PROXY=http://127.0.0.1:3128"
Environment="NO_PROXY=localhost,127.0.0.1,*.xxx.com"
```
重启服务
```bash
systemctl daemon-reload
systemctl restart docker
```