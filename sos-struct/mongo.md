# Mongo

## mongo 使用
```bash
mongo -u admin -p XXXXXX
# show dbs → tables;
```

## 数据导出
```bash
mongoexport -h 127.0.0.1 --authenticationDatabase admin -u root -p 'xxx' -d Dname -c Cname -f query -q '{}' --type=json -o Cname.json
```