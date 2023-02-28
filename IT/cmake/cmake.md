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

## CMakeLists.txt
```
# 设置工程的名称
# PROJECT(HELLO C CXX) # 支持C和C++
# PROJECT_BINARY_DIR == HELLO_BINARY_DIR == xx_BINARY_DIR
# PROJECT_SOURCE_DIR == HELLO_SOURCE_DIR == xx_SOURCE_DIR
PROJECT(HELLO)

# 指定变量的
# SET(SRC_LIST main.cpp t1.cpp t2.cpp)
SET(SRC_LIST main.cpp)

# 向终端输出⽤户⾃定义的信息
#	SEND_ERROR，产生错误，生成过程被跳过。
#	SATUS，输出前缀为—的信息。
#	FATAL_ERROR，立即终止所有 cmake 过程
MESSAGE(STATUS "This is BINARY dir " ${HELLO_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir "${HELLO_SOURCE_DIR})

# 生成可执行文件
# 第一个参数, 表示生成的可执行文件对应的文件名
# 第二个参数, 表示对应的源文件
ADD_EXECUTABLE(hello ${SRC_LIST})

# 向当前工程添加源文件子目录
# ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```

## 静态库与动态库构建
```
# ADD_LIBRARY 指令构建动态库和静态库
# SET_TARGET_PROPERTIES 设置目标属性
# 	同时构建同名的静态库和动态库。
# 	控制动态版本库

# 需要改变目标存放路径
# 定义在生成目标目录下的CMakeLists.txt
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

SET(LIBHELLO_SRC hello.cpp)

ADD_LIBRARY(hello SHARED ${LIBHELLO_SRC})
# 用于设置目标属性
# 在构建一个新的target时，会尝试清理掉其他使用这个名字的库
SET_TARGET_PROPERTIES(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)
# 增加动态库的版本号
SET_TARGET_PROPERTIES(hello PROPERTIES VERION 1.2 SOVERSION 1)

# ADD_LIBRARY(libname [SHARED|STATIC|MODULE][EXCLUDE_FROM_ALL] source1 source2 ... sourceN)
ADD_LIBRARY(hello_static STATIC ${LIBHELLO_SRC})
# target是不能重名的。否则会造成静态库的构建指令无效。
SET_TARGET_PROPERTIES(hello_static PROPERTIES OUTPUT_NAME "hello")
SET_TARGET_PROPERTIES(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
```