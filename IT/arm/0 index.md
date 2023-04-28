# 总流程
- 在主机上编译Bootloader,然后通过JTAG烧入单板
- 在主机上编译嵌入式Linux内核，通过Bootloader烧入单板或直接启动。
- 在主机上编译各类应用程序，单板启动内核后通过NFS运行它们，经过验证后再烧入单板。

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