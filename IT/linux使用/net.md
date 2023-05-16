- /etc/sysconfig/network-scripts/ifcfg-eth0 可以修改IP、子网掩码、广播地址、默认网关等
- /etc/rc.d/init.d/network restart 重启网卡

# ifconfig
- ifconfig eth0 x.x.x.x 对网卡进行设置, 临时生效
- ifconfig eth0 network x.x.x.x 对子网掩码设置, 临时生效


