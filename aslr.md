# Linux 虚拟地址问题
最近在使用pintool对spec程序进行采样时发现每次运行采出来的程序指令地址都不相同，这是由于Linux系统地址重定位(aslr)导致的。<br>
要解决这个问题，需要关闭aslr机制，方法如下:<br>
## 1. 使用 /proc/sys/kernel/randomize_va_space
可以先 cat /proc/sys/kernel/randomize_va_space 查看randomize_va_space的值，有0、1、2三种可能，如果没有进行修改的话值应该为2，每种对应的含义为：
- 0 – 不开启地址重定位，每次运行的虚拟地址相同。
- 1 – 部分重定位，共享的库、栈、堆、mmap函数、VDSO的地址是会改变的。
- 2 – 完全的重定位。
要关闭aslr，可以使用shell命令：
```shell
echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
```
再次开启
```shell
echo 2 | sudo tee /proc/sys/kernel/randomize_va_space
```
## 2. 添加配置文件
通过方法一改变的配置在重启后会恢复，所以需要长期关闭aslr可以添加文件/etc/sysctl.d/01-disable-aslr.conf，在其中加入
```
kernel.randomize_va_space = 0
```
就可以永久关闭了。

