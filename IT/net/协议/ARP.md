# ARP
- Address Resolution Protocol，即地址解析协议
- 负责把目的主机的IP 地址解析成目的MAC地址

## ARP协议报文
- ARP报文的总长度 64字节
	- 帧头 = 6+6+2 = 14
	- 以太帧 最小长度 46字节
		- ARP包头 = 2+2+4+6+4+6+4 = 28
		- PAD填充 = 18
	- CRC = 4
	
![](../../photo/Pasted%20image%2020221003215650.png)

![](../../photo/Pasted%20image%2020221003220417.png)
![ARP请求](../../photo/Pasted%20image%2020221003215227.png)
![ARP响应](../../photo/Pasted%20image%2020221003215324.png)

## 命令
arp -a

## ARP欺骗（ARP地址转换表）
1. ARP请求为广播形式发送的
2. 网络上的主机可以自主发送ARP应答消息
3. 主机收到应答报文时不会检测该报文的真实性就将其记录在本地的MAC地址转换表