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
