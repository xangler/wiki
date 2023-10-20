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
Environment="HTTP_PROXY=http://127.0.0.1:3128"
Environment="HTTPS_PROXY=http://127.0.0.1:3128"
Environment="NO_PROXY=localhost,127.0.0.1,*.xxx.com"
```
重启服务
```bash
systemctl daemon-reload
systemctl restart docker
```

## docker build 代理
```bash
docker build --network host -t demo:v1.0.0 -f Dockerfile .
```
Dockfile 新增代理设置
```text
ARG https_proxy=http://127.0.0.1:3128
ARG http_proxy=http://127.0.0.1:3128
```

## docker run 代理
```bash
docker run -it --rm \
--network=host \
--name=demo-os \
-e HTTP_PROXY=http://127.0.0.1:3128 \
-e HTTPS_PROXY=http://127.0.0.1:3128 \
-v ${PWD}:/work \
-w /work \
demo:v1.0.0 bash -c "sleep infinity"
```