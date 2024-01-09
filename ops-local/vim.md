# Vim 配置

## 配置文件
配置文件(${HOME}/.vimrc)
```bash
set number              "显示行号
set tabstop=4           "按下 Tab 键时，Vim 显示的空格数
set softtabstop=4       "Tab 转为多少个空格
syntax on               "自动语法高亮

set cursorline          "光标所在的当前行高亮。
set showmode            "在底部显示，当前处于命令模式还是插入模式
set ruler               "在状态栏显示光标的当前位置（位于哪一行哪一列）

set hlsearch            "搜索时，高亮显示匹配结果
set showmatch           "光标遇到圆括号、方括号、大括号时，自动高亮对应的另一个圆括号、方括号和大括号

set pastetoggle=<F9>    "切换paste模式
```