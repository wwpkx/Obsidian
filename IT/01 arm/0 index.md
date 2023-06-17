# 总流程
- 在主机上编译Bootloader,  然后通过**JTAG烧入**
	- **将BootLoader烧入到 nor flash中**, 并时候用nor flash启动（驱动、内核等都在**nand flash**中）
	- 然后再通过BootLoader操作其他烧入，比如内核，驱动
	- 从**nand flash启动**，测试驱动等
- 在主机上编译嵌入式Linux内核，通过Bootloader烧入
	- linux内核支持nfs
	- 然后通过nfs调试各种驱动或者软件
	- 通过后，再烧入

![](../photo/Pasted%20image%2020230428180306.png)

# arm 裸机
## arm裸机学习目的
- BootLoader
- 驱动开发，尤其是硬件驱动

## 学习方法
- 硬件
- 芯片手册
- 思维导图
- 程序设计
- 在线调试

## 裸机开发流程  
![](../photo/Pasted%20image%2020230421100823.png)
![](../photo/Pasted%20image%2020230421100919.png)

# ARM Linux的启动全过程
![](../photo/Pasted%20image%2020230424190444.png)