# Docker 配置

## 账号添加docker权限
```bash
sudo groupadd docker
sudo usermod -aG docker ${USER}
newgrp docker
```

## docker 管理
- systemctl status docker
- 配置查看: docker info
- docker配置文件: /etc/docker/daemon.json
- systemd配置文件: /etc/systemd/system/docker.service.d

## docker pull 代理
- systemd修改配置: /etc/systemd/system/docker.service.d/http-proxy.conf
```text
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:3128"
Environment="HTTPS_PROXY=http://127.0.0.1:3128"
Environment="NO_PROXY=localhost,127.0.0.1,*.xxx.com"
```

## docker build 代理
- docker build --network host -t demo:v1.0.0 -f Dockerfile .
- Dockfile 新增代理设置
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