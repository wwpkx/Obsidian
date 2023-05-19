- 在编译程序的时候使用‐g 选项生成调试信息

# 启动调试
- gdb <程序名> 
- gdb <程序名> core //程序非法执行后产生的 core dump 文件. 
- gdb <程序名> <pid> 指定进程号
- 件读取符号表
	- -symbols <file>
	- -s
	- -se
- core dump
	- -core <file>
	- -c <file>
- 源文件的搜索路径
	- -directory <dir>
	- -d <dir>
- 安静模式
	- -q

# GDB命令
| 命令                           | 描述         |
|------------------------------|------------|
| info breakpoints             | 列出断点。      |
| info break                   | 列出断点号。     |
| info break breakpoint-number | 列出指定断点的信息。 |
| info watchpoints             | 列出观察点。     |
| info registers               | 列出使用的寄存器。  |
| info threads                 | 列出当前的线程。   |
| info set                     | 列出可以设置的选项  |



