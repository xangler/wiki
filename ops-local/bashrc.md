# Bashrc 常用配置

## 修复sudo
部分操作系统sudo执行时找不到目标文件
```bash
alias sudo='sudo -E env "PATH=$PATH"
```

## 环境变量 MIP
```bash
export MIP=`ifconfig|grep inet|grep -v inet6|grep -v "127.0.0.1"|head -n 1|awk '{print $2}'`
```

## 跳板机 jump
```bash
export jump_ip='127.0.0.1'
alias jump='ssh -p 22 -i ${HOME}/.ssh/id_rsa rocky@${jump_ip}'
```

## 代理开关 proxy_on/off
```bash
export proxy_ip='127.0.0.1'
export proxy_port='55'
alias proxy_on='export http_proxy=http://${proxy_ip}:${proxy_port}/; export https_proxy=http://${proxy_ip}:${proxy_port}/; export HTTP_PROXY=http://${proxy_ip}:${proxy_port}/; export HTTPS_PROXY=http://${proxy_ip}:${proxy_port}/'
alias proxy_off='unset http_proxy; unset https_proxy; unset HTTP_PROXY; unset HTTPS_PROXY'
```

## 文件传输 szf
```bash
alias szf='UpMachine(){ echo "wget http://${MIP}:8080/${1}" && python -m SimpleHTTPServer 8080;};UpMachine'
```