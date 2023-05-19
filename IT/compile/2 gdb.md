- 在编译程序的时候使用‐g 选项生成调试信息

# 启动调试
- gdb <程序名> 
- gdb <程序名> core //程序非法执行后产生的 core dump 文件. 
- gdb <程序名> <pid> 指定进程号
- 读取符号表
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

| Break and Watch                    |                          |
|------------------------------------|--------------------------|
| break funtion                      | 在指定的函数，或者行号处设置断点。        |
| break line-number                  |                          |
| break +offset                      | 在当前停留的地方前面或后面的几行处设置断点。   |
| break -offset                      |                          |
| break file:func                    | 在指定的file文件中的func处设置断点。   |
| break file:nth                     | 在指定的file文件中的第nth行设置断点。   |
| break *address                     | 在指定的地址处设置断点。一般在没有源代码时使用。 |
| tbreak                             | 设置临时的断点。中断一次后断点会被删除。     |
| watch condition                    | 当条件满足时设置观察点。             |
| delete                             | 删除所有的断点或观察点。             |
| delete breakpoint-number           | 删除指定的断点，观察点。             |
| delete range                       |                          |
| disable breakpoint-number-or-range | 设置为无效，或有效。     |
| enable breakpoint-number-or-range  |                          |
| enable once breakpoint-number      | 设置指定断点有效，当到达断点时置为无效。     |
| enable del breakpoint-number       | 设置指定断点有效，当到达断点时删除它。      |
| finish                             | 继续执行到函数结束。               |



