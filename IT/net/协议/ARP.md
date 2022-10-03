# ARP
- Address Resolution Protocol，即地址解析协议
- 负责把目的主机的IP 地址解析成目的MAC地址

## 命令
arp -a

## ARP欺骗（ARP地址转换表）
1. ARP请求为广播形式发送的
2. 网络上的主机可以自主发送ARP应答消息
3. 主机收到应答报文时不会检测该报文的真实性就将其记录在本地的MAC地址转换表