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
- mm 修改内存，地址自动递增

