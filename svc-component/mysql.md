# Mysql

## mysql 使用
```bash
mysql -uroot -p xxxx
# show databases → tables;
```

## mysql 备份与恢复
```bash
mysqldump -h${MYSQL_ADDRESS} -P${MYSQL_PORT} -u${MYSQL_USERNAME} -p${MYSQL_PASSWORD} --databases ${MYSQL_DATABASES} --tables ${MYSQL_TABLES} --no-create-info --where="id in (1,2)" | gzip > demo.sql.gz

# mysql -h${MYSQL_ADDRESS} -P${MYSQL_PORT} -u${MYSQL_USERNAME} -p${MYSQL_PASSWORD} ${MYSQL_DATABASES} -e "select * from demo limit 1;"
mysql -h${MYSQL_ADDRESS} -P${MYSQL_PORT} -u${MYSQL_USERNAME} -p${MYSQL_PASSWORD} ${MYSQL_DATABASES} < demo.sql
```