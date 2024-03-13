# ssl 使用

## ssl 加密解密
```bash
# 加密
echo "demo" | openssl enc -aes-256-cbc -a -salt -pass pass:xdemo -pbkdf2
# 解密
echo "U2FsdGVkX1/fIZLA66/r47QhZ9MynpFpb9YVA0gMk2U=" | openssl enc -aes-256-cbc -a -d -salt -pass pass:xdemo -pbkdf2
```