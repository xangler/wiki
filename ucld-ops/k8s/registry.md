# registry使用教程

## registry 搭建
```bash
# 创建密钥
mkdir auth && htpasswd -Bbn demo demo > auth/htpasswd 
# 启动镜像
docker run -d \
--name=registry \
--restart always \
-p 5000:5000 \
-v ${PWD}/auth:/auth \
-e "REGISTRY_AUTH=htpasswd" \
-e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
-e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
-v /mnt/work/registry:/var/lib/registry \
registry
# 验证方式
curl -u demo:demo  http://localhost:5000/v2/
```