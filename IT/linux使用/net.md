- /etc/sysconfig/network-scripts/ifcfg-eth0 可以修改IP、子网掩码、广播地址、默认网关等
- /etc/rc.d/init.d/network restart 重启网卡
- service network restart 重启网卡

# ifconfig
- ifconfig eth0 x.x.x.x 对网卡进行设置, 临时生效
- ifconfig eth0 network x.x.x.x 对子网掩码设置, 临时生效

# netstat
- 显示网络统计信息
	- - p，表示显示哪个进程在调用

# traceroute
- 显示数据包经过历程

# route
- 显示路由表


