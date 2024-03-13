# selinux 使用

## selinux 管理
- 查看状态: sestatus
- 配置文件(修改需重启机器): /etc/selinux/config
- 日志记录: /var/log/audit/audit.log
- 审计排查: ausearch -m AVC,USER_AVC,SELINUX_ERR,USER_SELINUX_ERR -ts recent

## selinux 查看subject策略
```bash
ps -efZ
ls -alZ
getsebool -a
```

## selinux 新增subject(systemd)标签
```bash
sepolicy generate --init /usr/local/bin/demo
echo "logging_write_generic_logs(demo_t)" >> demo.te
echo "allow demo_t var_log_t:file { open write getattr };" >> demo.te
./demo.sh
restorecon -v /usr/local/bin/demo /usr/lib/systemd/system
```

## selinux 新增object标签
```bash
semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
restorecon -R -v /srv/web
```