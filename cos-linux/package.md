# package管理

## ubuntu离线包
```bash
#下载依赖包配置
apt-get --print-uris --yes install apache2 | grep ^\' | cut -d\' -f2 > todownload.txt
#下载依赖包
wget --input-file todownload.txt
#离线安装
dpkg -i *.deb
```