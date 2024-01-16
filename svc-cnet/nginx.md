# nginx 常规操作

## nginx 配置优化
```bash
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    # 开启brotli压缩； > gzip
    brotli on;
    brotli_comp_level 6;
    brotli_buffers 16 8k;
    brotli_min_length 20;
    brotli_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;
    server {
        listen       443 ssl;
        server_name  localhost;
        ssl_certificate      cert.pem;
        ssl_certificate_key  cert.key;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:20m;
        ssl_session_timeout 15m;
        ssl_session_tickets off;

        # 开启TLS1.3
        ssl_protocols TLSv1.2 TLSv1.3;
        # 开启ECC算法 > RSA 
        ssl_dyn_rec_enable on;
        ssl_ecdh_curve X25519:P-256;
        ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;

        # 开启 OCSP Stapling 当客户端访问时 NginX 将去指定的证书中查找 OCSP 服务的地址，获得响应内容后通过证书链下发给客户端。
        ssl_stapling on;
        ssl_stapling_verify on;# 启用OCSP响应验证，OCSP信息响应适用的证书
        ssl_trusted_certificate /path/to/xxx.pem;# 若 ssl_certificate 指令指定了完整的证书链，则 ssl_trusted_certificate 可省略。
        resolver 8.8.8.8 valid=60s;# 添加resolver解析OSCP响应服务器的主机名，valid表示缓存。
        resolver_timeout 2s;# resolver_timeout表示网络超时时间
        location / {
           root   html;
           index  index.html index.htm;
        }
    }
}
```