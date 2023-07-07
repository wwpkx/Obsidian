从0行代码开始，用约6000行代码实现一个小型的32位x86操作系统。

# 系统功能如下
- 支持多进程运行
- 支持shell加载磁盘上应用程序运行
- 支持虚拟内存管理，实现进程之间的隔离
- 键盘和显示器的支持
- 引用标准C库，使得应用程序开发更加方便
- 十余个系统调用：fork()、execve()、open()、write()、exit()等
- 进程与操作系统不同特权级分离

# 需要的软件
- x86_64-elf-gcc
- cmake
- qemu-system-i386
- dd --help (使用git中的)
- image目录
	- disk1.vhd
	- disk2.vhd
	- win_error.txt，报错时提示信息

# 代码目录
- code 源代码，每个目录都有运行
- start/start 测试程序，boot
- start/test 测试程序，完整程序
- image 镜像

# 调试
- vscode status bar，会生出调试功能项目
- 点击初始化gcc
- 点击build编译项目
	-  编译成功会有提示： `[build] Build finished with exit code 0`
- F5 调试 或者 左侧运行与调试窗口

![](../photo/Pasted%20image%2020230707185854.png)

![](../photo/Pasted%20image%2020230707185926.png)

# 注意
```
1. 
x86_64-elf-tools-windows\bin\x86_64-elf-objdump.exe: Dwarf Error: found dwarf version '5', this reader only handles version 2, 3 and 4 information.
这个报错可以忽略

2.
汇编如果不能断点
勾选 allow Breakpoints Everywhere
```