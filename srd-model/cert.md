# 数字证书(openssl)的使用
## 文件格式备注
> * pem(下文默认格式)文件前后带有BEGIN/END
> * der二进制格式, 命令行需带上 -infom der 
## 应用客户端
### 生成私钥(RSA PRIVATE KEY)
```bash
#生成密钥
openssl genrsa -out ca.key 2048
#查看密钥信息
openssl rsa -in ca.key -noout -text
```
### 生成公钥(PUBLIC KEY)
数字证书中不使用，其他场景可能会使用
```bash
#生成公钥
openssl rsa -in ca.key -pubout -out ca.pub
#查看公钥信息
openssl rsa -pubin -in ca.pub -noout -text
```
### 生成自签名证书(CERTIFICATE)
```bash
#生成自签名证书
openssl req -new -x509 -days 365 -key ca.key -out ca.crt -subj "/C=CN/ST=BJ/L=BJ/O=HD/OU=dev/CN=ca/emailAddress=ca@world.com"
#查看自签名证书
openssl x509 -in ca.crt -noout -text
```
## 应用服务器
## 生成私钥(略)
openssl genrsa -out api.key 2048
## 生成签名证书请求(CERTIFICATE REQUEST)
该证书不可使用, 需要去ca服务器获取真正的签名证书
```bash
#生成签名证书请求
openssl req -new -key api.key -out api.csr -subj "/C=CN/ST=BJ/L=BJ/O=HD/OU=ops/CN=*.joker.com/emailAddress=hello@world.com"
#查看签名证书请求
openssl req -in api.csr -noout -text
```
## CA服务器
### 生成私钥(略)
### 生成自签名证书(略)
### 应用服务器证书签名(CERTIFICATE)
```bash
openssl x509 -req -days 3650 -in api.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out api.crt
openssl x509 -in api.crt -noout -text
```