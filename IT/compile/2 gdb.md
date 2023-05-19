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

