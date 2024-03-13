# GUN 常用命令

## Gun Toolchain
- binutils
- gcc
- g++

## 编译原理
```bash
# 词法优化
clang -fsyntax-only -Xclang -dump-tokens a.c
# 语法分析
clang -fsyntax-only -Xclang -ast-dump a.c
# 语义分析(略)
# 中间代码生成
clang -S -emit-llvm a.c #a.ll
# 中间代码优化
clang -S -foptimization-record-file=- a.c -O1
clang -S -emit-llvm -debug-pass=Arguments a.c -O3 #查看优化项
# 目标代码生成
clang -S a.c --target=riscv64 -march=rv64g #a.s
clang -S a.c -ftime-report #查看优化内容
# 编译
clang -c a.c
# 链接(符号解析/重定位)
clang a.c
```

## 编译问题
- ABI规范
- C-LD问题
  - 库顺序(单趟解析)
  - name mangling(cpp)
  - 符号解析不检查类型
  - 同名试探性定义

## 编译优化
```bash
gcc -Q -O2 --help=optimizers #memory aliasing && inline function
# -fsanitize //UB+内存+线程锁
# -fno-elide-constructors //关闭返回值优化-->调试使用
```

## ELF文件解析
```bash
size xx                 #查看二进制数据大小
file xx.so              #查看动态库硬件架构
readelf -hSsr xxx.so     #查看elf文件头
objdump -h/-d/-s xx.so  #查看反汇编
ldd -r xx.so            #查看链接中 undefined symbol 
readelf -d xxx.so | grep NEEDED #查看交叉编译的链接问题
locate xxx.so                   #查看系统中的动态链接库
ldconfig -p | grep libnccl      #查看系统中的动态链接库
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

## 交叉编译
```c
#include <stdint.h>
void _start() {
  volatile uint8_t *p = (uint8_t *)(uintptr_t)0x10000000;
  *p = 'A';
  while (1);
}
```
```bash
riscv64-linux-gnu-gcc -ffreestanding -nostdlib -Wl,-Ttext=0x80000000 -O2 a.c
qemu-system-riscv64 -nographic -M virt -bios none -kernel a.out
```
