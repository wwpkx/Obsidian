## Bootloader的作用
![](../photo/Pasted%20image%2020230422184024.png)

## U-Boot简介
- 用于**多种嵌入式CPU**（ MIPS、x86、ARM等）的**bootloader程序**
- 不仅支持嵌入式Linux系统的引导，还支持VxWorks, QNX等**多种嵌入式操作系统**
- 模式
	- 自主模式
	- 开发模式

# U-Boot工作流程分析

![](../photo/Pasted%20image%2020230428183638.png)
![](../photo/Pasted%20image%2020230428183749.png)

# u-boot使用
![](../photo/Pasted%20image%2020230502101818.png)
```
U-Boot使用方法
1. 配置U-Boot 
TQ210:    make TQ210_config
Smart210:   make smart210_config
OK210:     make forlinx_linux_config
OK6410:    make forlinx_nand_ram256_config
Tiny6410:   make tiny6410_config
TQ2440:   make TQ2440_config
Mini2440:   make mini2440_config

2. 下载与运行
TQ210:    tftp 0xc0008000 uImage
Smart210:  tftp 0x20000000 uImage
OK210:     tftp 0xc0008000 uImage 
OK6410:    tftp 0xc0008000 uImage
Tiny6410:   tftp 0xc0008000 uImage
TQ2440:    tftp 0x31000000 uImage
Mini2440:   tftp 0x31000000 uImage
```
## 编译
- 为了方便编译，可以配置**Makefile文件中的ARCH和CROSS_COMPILE**变量的值
```
// 清除工程
// ARCH 指定架构
// CROSS_COMPILE 用于指定编译器, arm-linuxgnueabihf-gcc
# make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean  

// 配置 uboot
// mx6ull_14x14_ddr512_emmc_defconfig 是配置文件，由恩智浦官方提供（在路径uboot/configs）
// 在根目录生成一个.config文件
# make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_ddr512_emmc_defconfig

// 编译uboot
// V=1 用于设置编译过程的信息输出 级别
// -j 用于设置主机使用多少线程编译uboot
// 编译完成后会产生.bin文件，在编译中会添加I.MX6ULL的头部信息产生.imx文件
make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j1
```

## 环境变量
- printenv 查看环境变量
- setenv 添加、修改、删除环境变量
- saveenv 保存环境变量
	- 将当前定义的所有变量及其值存入flash中
```
#printenv
#printenv ipaddr
#setenv myboard 210	#add/modify
#setenv myboard  	#delete
```
## 文件下载
![](../photo/Pasted%20image%2020230502102640.png)

## 执行程序
![](../photo/Pasted%20image%2020230502102726.png)

## 查看内存内容
- md 显示内存区的内容
	- md [.b, .w, .l] address  //采用长度标识符 .l, .w和.b
- mm 修改内存，地址自动递增
	- mm [.b, .w, .l] address
	- 一种互动修改存储器内容的方法
	- 按回车，该地址的内容保持不变
	- 空格+回车, 结束输入

```
md.w 100000
00100000: 2705 1956 5050 4342 6f6f 7420 312e 312e 
00100010: 3520 284d 6172 2032 3120 3230 3032 202d

mm 100000
00100000: 27051956 ? 0
00100004: 50504342 ? AABBCCDD
```

## nand flash操作
- 擦除nand flash
	- nand erase 起始地址start 长度len
- 写 nand flash
	- nand write 内存起始地址 flash起始地址 长度len   // 内存-->nand flash
- 读 nand flash
	- nand read 内存起始地址 flash起始地址 长度len  // nand flash-->内存
```
#nand erase 0x400000 0x500000
#nand write c0008000 400000 500000  // 内存-->nand flash
#nand read c0008000 400000 500000   // nand flash-->内存
```
## 设置自启动
![](../photo/Pasted%20image%2020230502103704.png)
