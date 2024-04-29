# Index 1


asdf
aads

# dss
## dsdds
# ss



---

## Index 1 1


asdf
aads

# dss
## dsdds
# ss



---

## Index 1 1 1


asdf
aads

# dss
## dsdds
# ss



---

# 1 gcc



---

## 1.1 gcc概述

- GNU Compiler Collection
-  **1987年发布了第一个C语言版本**
-  支持多种不同的硬件平台，如x86、 ARM等体系结构。
- 查看 gcc 的版本：gcc –v

# 编译过程分为 4 个阶段
- 预处理, Preprocessing
- 编译, Compilation
- 汇编, Assembly
- 链接, Linking
![](../photo/paste-809c66b2f581233ac83c71cfdf54902574da527b.jpg)

# 扩展名

- .c    需要预处理的 C 语言源代码
- .h    C 语言源代码头文件
- .i    不需要预处理的 C 语言源代码
- .s    汇编语言代码
- .o    目标文件
- .a    静态对象库
- .so   共享对象库

---

## 1.2 gcc编译

- 默认头文件：/usr/include/
- 默认库文件：/usr/lib/

# ARM GCC 
- 本地编译：当前平台编译，在当前平台运行
- 交叉编译：**当前平台编译，在其他平台运行**。原因，arm处理器性能低下

# 编译选项
`gcc [选项] 源文件 [选项] 目标文件 `
- -o    指定输出文件名称
- -E    只进行预处理
- -S    只进行预处理、编译
- -c    只预处理、编译、汇编，但不链接
- -D    `预定义宏；-D name[=definition]预定义名为name的宏，若不指定值则默认宏的内容为1`
- -l    查找具体的库
- -static   指定静态链接(默认是动态链接)
- -L        查找库的目录。系统的库目录 + -L指定的目录下
- -I        查找头文件目录
- -include  等效于在被编译的源文件开头添加 `#include "file"`
- -O0~3	  开启编译器优化，-O0为不优化，-O3为最高级别的优化
- -Os	  优化生成代码的尺寸
- -Og	  优化调试体验，在保留调试信息的同时保持快速的编译
- -Wall	  使编译器输出所有的警告信息
- -march	  指定目标平台的体系结构，如-march=armv4t，常用于交叉编译
- -mtune	  指定目标平台的CPU以便GCC优化，如-mtune=arm9tdmi，常用于交叉编译
- ‐g             包含调试信息

gcc -o hello hello.c **-I** /home/hello/include **-L** /home/hello/lib **-l**world
- -I /home/hello/include ，查找头文件的目录
- -L /home/hello/lib，查找寻找库文件的目录
- -lworld，查找库文件
	- 寻找**libworld.so**动态库文件
	- -static 寻找**libworld.a**静态库文件

# 编译环境变量
- 目录的分隔符为分号（windows），或者为冒号（linux）。
- 查找头文件
	- CPATH，-I选项
	- C_INCLUDE_PATH，查找c头文件
	- CPLUS_INCLUDE_PATH，查找c++头文件
- 查找库
	- LIBRARY_PATH，静态库
	- LD_LIBRARY_PATH，动态库

---

### 1.2.1  编译程序

# 编译单源程序
语法：`gcc [选项参数] c 文件`
```
例子：gcc ch01.c

1、指定输出文件名
-o 指定输出文件名
例子：gcc -o main ch01.c

2 、警告与提示.
-pedantic 检测不符合ANSI/ISO C语言标准的源代码,使用扩展语法的地方将产生警告信息。
-Wall 生成尽可能多的警告信息。
-Werror 要求编译器将警告当做错误进行处理。
例子：gcc –Wall –o main ch01.c

3 、指定编译文件类型
-x 指定编译代码类型，c、c++、assembler，none。None 根据扩展名自动确认。
例子：gcc –x c -Wall –o main ch01.c

4、生成调试信息与优化
-g 生成调试信息
-O 优化

5、建议：在编译任何程序的时候都带上-Wall选项。
gcc -x c -o main -Wall ch01.c
```

# 编译多源程序
```
语法：gcc [选项] C 源代码 1 C 源代码 2 C 源代码 3
gcc -x c -o main -Wall ch01.c ch01_1.c
```

# 编译多个目标文件
```
语法：gcc –o 输出文件名 目标文件1 目标文件2
gcc -o main ch01.o ch01_1.o
```

---

### 1.2.2 预处理指令

# 预定义宏
```
__cplusplus    C++有效
__DATE__       日期
__TIME__       时间
__BASE_LINE__  源代码的完整路径
__FILE__       源代码文件名
__func__       当前函数名
__FUNCTION__   同上
__LINE__       行数
__INCLUDE_LEVEL__ 包含层数,基本的为0
```

# 预处理指令
- 在C语法中引入很多预处理指令，这些指令影响到 gcc 的预编译处理结果。
```
#define           定义宏
#elif             else if 多选分支
#else             与#if、#ifndef、#ifdef 结合使用
#error            产生错误,挂起预处理程序
#if               判定
#endif            结束判定
#ifdef            判定宏是否定义
#ifndef           判定宏是否定义
#include          将指定的文件插入#include的位置
#include_next     与#include一样,但从当前目录之后的目录查找
#line             指定行号
#pragma           提供额外信息的标准方法，可用来指定平台
#undef            删除宏
#warning          创建一个警告
##                连接操作符号，用于宏内连接两个字符串
```
