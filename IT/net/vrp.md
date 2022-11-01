# VRP(Versatile Routing Platform,通用路由平台)
- **华为**公司具有**完全自主知识产权**的**网络操作系统**
- 可用运行**多硬件平台**之上(路由器、交换机、防火墙)
- 拥有**一致**的网络界面、用户界面和管理界面，提供丰富的应用解决方案
- 集成了**路由交换技术、QOS技术、安全技术和IP语言技术等**数据通信功能
- 主要使用版本：**VRPv5,** **高级版本v8**
- 路由器的缺省用户名为：admin 密码为：Admin@huawei

## VRP命名规则
-   由VRP**自身版本号和关联产品版本号**两部分组成
-   产品版本格式包含 **设备型号， Vxxx （产品码），Rxxx（大版本号），Cxx（小版本号），SPCxx（补丁）**
	- Version **5.130自身版本号** (**AR2200设备型号** **V200产品码**  **R003大版本号**  **C00小版本号**)
	- Version **5.110自身版本号**(**S5700设备型号** **V200产品码**  **R001大版本号**  **C00小版本号**)
	- ar2220-v200r003c00spc200.cc

![](../photo/Pasted%20image%2020220928171735.png)
![](../photo/Pasted%20image%2020220928171842.png)

# 设备内存
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

# 远程管理

## 命令行视图
| 用户界面类型   | 编号  |
|----------|-----|
| Console  | 0   |
| VTY      | 0-4 |

## 配置视图
- 用户视图
	- 提示符如下：**< Huawei >**
	- 登录后，系统默认为用户视图
	- 查看运行状态等，如进行时钟设置，系统重启，恢复出厂设置等
- 系统视图
	- 提示符如下：**[ Huawei ]**
	- 在用户模式下，通过system-view进入系统视图
	- 配置设备的系统参数，如：设备名称，aaa认证、acl配置等等；
- 接口视图
	- 提示符如下：**[HUAWEI-GigabitEthernet0/0/1]**
	- 在系统视图下，通过接口命令**interface gigabitethernet 0/0/1**进入
	- 配置接口参数；如何接口重启或关闭
- 协议视图
	- 提示符如下：**[ Huawei-ospf-1 ]**
	- 与接口视图平级，由系统视图进入


## 用户等级
> 用户等级 和 配置视图 **没有关系**，比如reboot属于用户视图 和 3管理级

| 用户等级 | 命令等级        | 名称  |
|------|-------------|-----|
| 0    | 0           | 访问级 |
| 1    | 0 and 1     | 监控级 |
| 2    | 0,1 and 2   | 配置级 |
| 3-15 | 0,1,2 and 3 | 管理级 |

## VTY（命令行终端） 
- 虚拟类型终端（Virtual Type Terminal）是一种虚拟线路端口
- 用户通过终端与设备建立Telnet或SSH连接后，也就建立了一条VTY
- 设备一般最多支持15个用户同时通过VTY方式访问
- 路由器上有5个VTY口，分别0、1、2、3、4
- 如果想同时配置这5个口，line vty 0 4
```
(config)#display user-interface //命令用来查看用户界面信息。
(config)#user-interface maximum-vty number //配置同时登录到设备的VTY类型用户界面的最大个数。如果设为0，则任何用户都不能通过Telnet或者SSH登录到路由器。

(config)#line vty 0 ? //查看该设备支持多少条线路。
(config)#line vty 0
(config-line)#transport ?   //查看支持哪种方式的协议定义
```

## 认证模式
| 认证模式     | 描述 |
| ------------ | ---- |
| AAA模式      |   用户名和密码   |
| 密码认证模式 |   所有的用户使用的都是同一个密码   |
| 不认证模式             |   Console界面默认使用   |

| Telnet server enable                         | 开启telnet服务 |
|----------------------------------------------|------------|
| Display telnet server                        | 验证telnet服务 |
|  user-interface vty 0 4                          | 进入VTY配置模式  |
|  set authentication-mode password/aaa        | 配置认证模式     |
|          user privilege level 15             | 配置用户权限     |
|          user-interface maximum-vty 15       | 配置最大接入数    |
|          idle-timeout 10                     | 配置超时时间     |
| AAA                                          | 进入AAA配置模式  |
| local-user huawei password cipher huawei@123 | 创建用户名和密码   |
| local-user huawei privilege level 15         | 配置用户权限     |
| local-user huawei service-type telnet        | 配置服务类型     |

```
//none认证模式
[AR1]user-interface vty 0 4/*同时设置5个vty连接，可同时供5个telnet连接此设备，当超出5个时，无法连接*/
[AR1-ui-vty0-4]set authentication none/*none认证模式，telnet连接时无需输入密码*/

//Password模式
[AR1]user-interface vty 0 4
[AR1-ui-vty0-4]set authentication password cipher huawei/*设置成密码连接方式*/

//AAA认证
[AR1]aaa
[AR1-aaa]local-user huawei password cipher 123
Info: Add a new user.
[AR1-aaa]local-user huawei service-type telnet /*设置账号的服务类型*/
[AR1-aaa]local-user huawei privilege level 3 /*设置远程登录后的账号权限等级*/
[AR1-aaa]q
[AR1]user-interface vty 0 4
[AR1-ui-vty0-4]authentication-mode aaa/*设置vty的认证模式为aaa认证*/
```

# 命令
所有命令都可以参考[官网](https://support.huawei.com/enterprise/zh/doc/EDOC1100222990/fd741f79)
```
设备初始化启动，询问是否进入**自动配置（一问一答模式）**，一般选择手动配置
Do you want to **stop** Auto-Config? [y/n]:Y
```

## 常用命令
- delete 将文件删除到回收站
- unreserved 永久删除文件
- Undelete 恢复删除的文件
- Reset recycle-bin 清空回收站
- reboot 重启设备
- display h?  在线帮助
- fixdisk flash/sd1 存储设备修复，执行后，如果仍收到建议修复信息，物理介质可能已经损坏
- format flash/sd1 格式化，格式化会导致数据丢失

## 各种查看命令
- display version 查看路由器基本信息 
- display interface GigabitEthernet 0/0/0 查看接口状态信息
- display ip interface brief 查看全部接口的IP简要信息，含IP地址
- display ip routing-table 查看路由表
- display current-configuration 查看当前的配置（内存中）
- display saved-configuration 查看保存的配置（Flash中）
- dir flash: 查看Flash中的文件

## 配置标题消息
- header login 配置在用户登陆前显示的标题消息
- header shell 配置在用户登陆后显示的标题消息

## 配置时钟
- clock timezone 设置所在时区
- clock datetime 设置当前时间和日期
- clock daylight-saving-time 设置采用夏时制

## 配置用户界面命令
- idle-timeout 设置超时时间
- screen-length 设置指定终端屏幕的临时显示行数
- history-command max-size 设置历史命令缓冲区的大小

## 配置系统界面命令
- sysname xxx 配置设备名称，立即生效

## 配置信息
**display startup** 命令用来查看设备本次及下次启动相关的系统文件和版本。
- 系统软件, .cc
- 配置文件, vrpcfg.zip
- 补丁文件, .pat

```
显示本次及下次启动相关的文件名。

<HUAWEI> **display startup**

  Startup system software:                   flash:/basicsoftware.cc
  Backup startup system software:            flash:/basicsoftware.cc
  Next startup system software:              flash:/basicsoftware.cc
  Startup saved-configuration file:          flash:/vrpcfg.zip
  Next startup saved-configuration file:     flash:/vrpcfg.zip
  Startup patch package:                     NULL
  Next startup patch package:                NULL
```

### 系统文件 【系统软件必须存放在存储器的根目录下】
- **startup system-software** _system-file_
- **startup system-software** { **backup** | **current** }

### 配置文件
- display startup 查看系统启动配置参数
- display current-configuration 显示当前配置文件 
- display saved-configuration 显示保存的配置文件
- 
- reset saved-configuration 配置文件重置
- save [configuration-file] 保存当前配置信息，默认文件名为**vrpcfg.zip**
- startup saved-configuration  [configuration-file]  配置**系统下次启动时使用的**配置文件
- 
- compare configuration 比较当前配置和保存的配置

### 补丁文件
- **startup patch** _patch-name_  命令用来指定设备下次启动时使用的补丁文件。
- **patch delete** 命令删除当前补丁
- **reset patch-configure next-startup** 命令清除设备重启后启用的补丁包文件

