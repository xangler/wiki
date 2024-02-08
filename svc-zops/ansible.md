# ansible操作

## ansible多个节点执行命令
```bash
ansible all -m shell -a "df -lh|grep mysql"
```