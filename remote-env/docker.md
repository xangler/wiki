# Docker 配置

## 账号添加docker权限
```bash
sudo groupadd docker
sudo usermod -aG docker ${USER}
newgrp docker
```