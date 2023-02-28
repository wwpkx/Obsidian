# 说明
- 不同IDE集成的**make遵循的规范和标准不同**，导致其语法、格式不同
- 跨平台，cmake的工具链：cmake+make
- 每个目录一个CMakeLists.txt

![](../photo/Pasted%20image%2020230228094352.png)

# 语法
- 变量使用 ${} 取值
- 指令(参数 1 参数 2...)
- 指令不区分大小写（推荐大写），参数和变量区分大小写

# 静态库和动态库的区别  
- 静态库的扩展名⼀般为“.a”或“.lib”；动态库的扩展名一般为“.so”或“.dll”。  
- 静态库在编译时会直接整合到目标程序中，编译成功的可执文件可独立运行
- 动态库在编译时不会放到连接的目标程序中，即可执文件无法单独运行

# 使用
- 写CMakeLists.txt
- 使用命令 **cmake .**  //目的，生成makefile文件
	 - 内部构建
		- 编译文件与源文件混杂在一起
	- **外部构建（推荐）**
		- 新建build文件夹，在build文件夹中执行 **cmake ..**
		- `PROJECT_SOURCE_DIR`，工程路径，/work
		- `PROJECT_BINARY_DIR`， 编译路径，/work/build。
- 使用命令 **make**
- 使用命令 **make install**
	- make install DESTDIR=/tmp/test
	- ./configure –prefix=/usr

## 外层CMakeLists.txt

## src下的CMakeLists.txt