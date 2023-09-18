# Cpp 常用命令

## symbol lookup error
```bash
size xx                 #查看二进制数据大小
file xx.so              #查看动态库硬件架构
ldd -r xx.so            #查看链接中 undefined symbol 
readelf -d xxx.so | grep NEEDED #查看交叉编译的链接问题
objdump -h/-d/-s xx.so
ldconfig -p | grep libnccl #查看系统中的动态链接库
```

## 进程调用卡住分析工具
```bash
cd /proc/4795           #fd status stack //查看系统记录
pstack 4795             #查看进程栈
starce -p 4795          #查看系统调用
lsof -d 1               #通过fd进行查询进程
lsof -p 4795            #通过进程id查询fd
```


## GDB调试
```bash
gdb main                #有源码调试 常用命令 l b r c p bt
set follow-fork-mode parent|child #多进程模式:fork之后继续调试父进程还是子进程
set detach-on-fork on|off  #多进程模式:gdb将控制父进程和子进程
info inferiors          #列出所有进程
inferior id             #切换到指定进程
detach inferior ID      #detach指定的进程

gdb -p 4795             #attach 4795 //运行调试
set print thread-events #多线程模式:控制打印线程启动或结束是的信息
set scheduler-locking off|on|step #多线程模式:在s或c进行调试的时候线程并行执行方式
thread thread-id        #切换到指定线程
info threads            #列出所有线程
thread apply [thread-id-list] [all] args    #执行命令
```
