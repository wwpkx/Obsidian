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
| Source Code              |                    |
|--------------------------|--------------------|
| **where**                    | 显示当前的行号和所处的函数。     |
| **list**                     | 列出相应的源代码。          |
| set listsize count       | 设置list命令打印源代码时的行数。 |
| show listsize            |                    |
| directory directory-name | 在源代码路径前添加指定的目录。    |
| show directories         |                    |
| directory                | 当后面没有参数时，清除源代码目录。  |

| info                           | 描述         |
|------------------------------|------------|
| info breakpoints             | 列出断点。      |
| info break                   | 列出断点号。     |
| info break breakpoint-number | 列出指定断点的信息。 |
| info watchpoints             | 列出观察点。     |
| info registers               | 列出使用的寄存器。  |
| info threads                 | 列出当前的线程。   |
| info set                     | 列出可以设置的选项  |

| Start and Stop  |                                         |
|-----------------|-----------------------------------------|
| run/r           | 从头开始执行程序，也允许进行重定向。                      |
| continue/c      | 继续执行直到下一个断点或观察点。                        |
| continue number | 继续执行，但会忽略当前的断点number次。当断点在循环中时非常有用。     |
| kill            | 停止程序执行。                                 |
| step            | 进入下一行代码的执行，会进入函数内部。                     |
| next            | 执行下一行代码。但不会进入函数内部。                      |
| until           | 继续运行直到到达指定行号，或者函数，地址等。                  |
| return          | 弹出选中的栈帧（stack frame）。如果后面指定参数，则返回表达式的值。 |
| stepi           | 执行下一条汇编/CPU指令。                          |

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

| Variables   |                         |
|---------------------|-------------------------|
| print variable      | 打印指定变量的值。               |
| p file::variable    |                         |
| p *array-var@length | 打印arrary-var中的前length项。 |
| **p/x var**             | 以十六进制打印整数变量var。         |
| **p/o var**             | 把变量var作为八进制数打印。         |
| **p/t var**             | 以整数二进制的形式打印var变量的值。     |
| **x/w address**         | 打印指定的地址，以四字节一组的方式。      |
| x/b address         |                         |
| p/c variable        | 当字符打印。                  |
| p/d var             | 把变量var当作有符号整数打印。        |
| p/u var             | 把变量var作为无符号整数打印。        |
| p/f variable        | 以浮点数格式打印变量var。          |

| Stack           |                                         |
|-----------------|-----------------------------------------|
| backtrace/bt    | 显示当前堆栈的追踪，当前所在的函数。                      |
| backtrace full  | 打印所有局部变量的值。                             |
| frame/f number  | 选择指定的栈帧。                                |
| up/down number  | 向上或向下移动指定个数的栈帧。                         |
| info frame addr | 描述选中的栈帧。                                |
| info args       | 显示选中栈帧的参数，局部变量，异常处理函数。all-reg也会列出浮点寄存器。 |
| info all-reg    |                                         |
| info locals     |                                         |
| info catch      |





