# Tmux 配置

## Powerline
[安装文档](https://powerline.readthedocs.io/en/latest/installation.html)

## 配置文件
配置文件(${HOME}/.tmux.conf)
```bash
set-option -g prefix C-x
unbind-key C-b 
bind-key C-x send-prefix
setw -g mode-keys vi
bind-key R source-file ~/.tmux.conf
bind-key D source-file ~/.tmux.init
bind-key F setw synchronize-panes
```
初始化文件(${HOME}/.tmux.init)
```bash
rename-window local
split-window -v
selectp -t 0
split-window -h
selectp -t 2
split-window -h
selectp -t 0
neww -n middle
split-window -v
selectp -t 0
split-window -h
selectp -t 2
split-window -h
selectp -t 0
neww -n remote 
split-window -v
selectp -t 0
split-window -h
selectp -t 2
split-window -h
selectp -t 0
selectw -t 0
```
