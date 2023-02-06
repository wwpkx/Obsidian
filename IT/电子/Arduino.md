- Arduino是电子原型平台
	- 硬件（各种型号的 Arduino板）
	- 软件（[Arduino IDE](https://www.arduino.cc/en/software)）
		- 源码后缀，.ino
		- 使用c/c++语法

- Arduino IDE - win
	- 可以直接使用zip包
	- 安装驱动
		- 连接板子
		- 在设备管理中，找到新的硬件（浏览计算机以查找驱动程序软件）
		- 在解压文件中找到 **ArduinoUno.inf** 文件

-  串口通信
	- 可以和 [Processing](../Processing/Processing.md) 配合编程
	- 平时电脑连接板子时，使用的串口就是**引脚0(RX Receive)和引脚1(TX Transmit)**
	-  还可以使用**软件模拟串口SoftSerial**，它使用两个输出引脚