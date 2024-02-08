# VScode 配置

## vscode命令行启动
* 打开 VSCode。
* 按下“Ctrl+Shift+P”，输入“Shell Command: Install 'code' command in PATH”，然后按下回车键。
* 关闭 VSCode。
* 在命令提示符中输入“code”，按下回车键，即可启动 VSCode。

## 远程开发
```bash
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host jump
    HostName 127.0.0.1
    Port 22
    User rocky
    IdentityFile /xxx/.ssh/id_rsa

Host DEVL
    HostName 127.0.0.1
    User rocky
    Port 22
    ProxyCommand ssh -W %h:%p jump

Host DEVH
    HostName 127.0.0.1
    User rocky
    Port 22
    ProxyJump jump
```