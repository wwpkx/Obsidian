# VRP(Versatile Routing Platform,通用路由平台)
- **华为**公司具有**完全自主知识产权**的**网络操作系统**
- 可用运行**多硬件平台**之上(路由器、交换机、防火墙)
- 拥有**一致**的网络界面、用户界面和管理界面，提供丰富的应用解决方案
- 集成了**路由交换技术、QOS技术、安全技术和IP语言技术等**数据通信功能
- 主要使用版本：**VRPv5,** **高级版本v8**
- 路由器的缺省用户名为：admin 密码为：Admin@huawei

## VRP命名规则
-   由VRP**自身版本号和关联产品版本号**两部分组成
-   产品版本格式包含**设备型号 Vxxx （产品码），Rxxx（大版本号），Cxx（小版本号）**
	- Version **5.130自身版本号** (**AR2200设备型号** **V200产品码**  **R003大版本号**  **C00小版本号**)
	- Version **5.110自身版本号**(**S5700设备型号** **V200产品码**  **R001大版本号**  **C00小版本号**)
-   如果VRP产品版本有补丁，VRP产品版本中还包含SPC部分

![](../photo/Pasted%20image%2020220928171735.png)
![](../photo/Pasted%20image%2020220928171842.png)

# 设备管理方式
| 管理方式   | 登入方式    | 优点                   | 缺点       | 应用场景                        |
|--------|---------|----------------------|----------|-----------------------------|
| CLI命令行 | console | 使用console线缆连接 完全本地管理 | 单会话 无法远程 | 初始化 故障恢复 升级。**serial协议**        |
| CLI命令行 | miniUSB | 使用miniUSB线缆连接 完全本地管理 | 单会话 无法远程 | 初始化 故障恢复 升级。serial协议        |
| CLI命令行 | Telnet  | 远程管理 多会话             | 明文传输 不安全 | 对安全性要求不高的网络                 |
| CLI命令行 | SSH     | 远程管理  多会话 高安全性       | 配置较复杂    | 对安全性要求高的网络                  |
| web图形化 | HTTP    | 图形化界面，更直观            |          | 登入使用HTTPS(SSL证书)，传输数据使用HTTP |
| web图形化 | HTTPS   | 图形化界面，更直观            |          | 登入和传输数据都是用HTTPS，需要开启命令      |

![](../photo/Pasted%20image%2020220928173139.png)

![](../photo/Pasted%20image%2020220928173128.png)
![](../photo/Pasted%20image%2020220928180158.png)


![](../photo/Pasted%20image%2020220928173132.png)

![](../photo/Pasted%20image%2020220928173148.png)

![](../photo/Pasted%20image%2020220928173200.png)

# 文件系统
## 设备内存
- RAM        随机访问存储器（Random Access Memory，RAM），断电之后信息就丢失了
- SDRAM   内存 （临时性存储） 断电后配置文件丢弃 （内存较大）
- 
- NVRAM   内存   断电后配置文件还在 （内存小）
	- NVRAM（ Non-Volatile Random Access Memory） 是非易失性随机访问存储器
	- 指断电后仍能保持数据的一种RAM
- 
- Flash       闪存    （永久性存储）
	- nand flash，属于顺序访问（serial access）
	- nor flash，属于是可以随机访问的
- 
- SD card  SD卡 （永久性存储）
- USB  （移动存储）

# 命令
- Delete 将文件删除到回收站
- unreserved 永久删除文件
- Undelete 恢复删除的文件
- Reset recycle-bin 清空回收站
- 
- fixdisk flash/sd1 存储设备修复，执行命令后，如果仍然收到系统建议修复的信息，则表示物理介质可能已经损坏。
- format flash/sd1 格式化，格式化会导致数据丢失


## FTP与TFTP的区别


## 配置文件
- display startup 查看系统启动配置参数
- display current-configuration 显示当前配置文件 
- display saved-configuration 显示保存的配置文件
- 
- reset saved-configuration 配置文件重置
- save [configuration-file] 保存当前配置信息，默认文件名为**vrpcfg.zip**
- startup saved-configuration  [configuration-file]  配置**系统下次启动时使用的**配置文件
- system-software  设置下一启动的系统文件
- 
- patch //Set patch file
- compare configuration 比较当前配置和保存的配置

## 系统配置
	设备初始化启动，询问是否进入**自动配置（一问一答模式）**，一般选择手动配置
	Do you want to **stop** Auto-Config? [y/n]:Y
	
	<Huawei> display h?  //在线帮助


![命令行视图](../photo/Pasted%20image%2020220929093706.png)



| 命令                                    | 功能                               |
| --------------------------------------- | ---------------------------------- |
| clock timezone                          | 设置所在时区                       |
| clock datetime                          | 设置当前时间和日期                 |
| clock daylight-saving-time              | 设置采用夏时制                     |
| display version                         | 查看路由器基本信息                 |
| display interface GigabitEthernet 0/0/0 | 查看接口状态信息                   |
| display ip interface brief              | 查看全部接口的IP简要信息，含IP地址 |
| display ip routing-table                | 查看路由表                         |
| display current-configuration           | 查看当前的配置（内存中）           |
| display saved-configuration             | 查看保存的配置（Flash中）          |







