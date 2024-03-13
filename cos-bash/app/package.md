# package管理

## rocky自制yum源
- 官方源目录
    - mount rhel-dvd.iso /var/rhel/
- 自制源目录
    - 创建目录，将所有rpm包保存其中
    - 目录中执行createrepo即可

## ubuntu离线包
```bash
#下载依赖包配置
apt-get --print-uris --yes install apache2 | grep ^\' | cut -d\' -f2 > demo.txt
#下载依赖包
wget --input-file demo.txt
#离线安装
dpkg -i *.deb
```