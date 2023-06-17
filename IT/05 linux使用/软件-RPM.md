- Redhat Package Manager（Redhat软件包管理工具）
- apache-1.3.23-11.i386.rpm
	- “apache”：软件名称
	- “1.3.23-11”：软件的版本号，主版本和次版本
	- “i386”：是软件所运行的硬件平台
	- “rpm”：文件扩展名，代表RPM包

# RPM包安装位置
- /etc/              配置文件安装目录
- /usr/bin/          可执行的命令安装目录
- /usr/lib/          程序所使用的函数库保存位置
- /usr/share/doc/    基本的软件使用手册保存位置
- /usr/share/man/    帮助文件保存位置

# RPM包安装的服务
- 可以使用**系统服务管理命令（service）**来管理
	- /etc/rc.d/init.d/httpd start
	- service httpd start

# RPM包依赖性
- 树形依赖： abc
- 环形依赖： abca
- 模块依赖： 模块依赖查询网站：
	- www.rpmfind.net

# 常用命令
```
安装RPM包
rpm ‐ivh RPM包全名
	-i=install，安装
	-v=verbose，提示，即有提示信息
	-h=hash，进度条
	--nodeps 不检测依赖性

rpm -Uvh 包全名  
	-U（upgrade） 升级

查询
rpm ‐qa：查询所安装的所有rpm软件包
	-rpm ‐qa | more
	-rpm ‐qa | grep X
rpm ‐q 软件包名：查询软件包是否安装
	-rpm ‐q xinetd
	-rpm ‐q foo
rpm ‐qi 软件包名：查询软件包信息
	-rpm ‐qi file	
rpm ‐ql 软件包名：查询软件包中的文件
	-rpm ‐ql file
	-rpm ‐ql jdk
rpm ‐qf 文件全路径名：查询文件所属的软件包
	-rpm ‐qf /etc/passwd
	-rpm ‐qf /root/install.log
rpm ‐qp 包文件名：查询包的信息对这个软件包的介绍
	-rpm ‐qp jdk-1_5_0-linux-i586.rpm
	-rpm ‐qpi jdk-1_5_0-linux-i586.rpm
	-rpm ‐qpl jdk-1_5_0-linux-i586.rpm

删除RPM包
rpm ‐e RPM包的名称
	-e（erase） 卸载
	--nodeps 不检查依赖性
```
# RPM包校验
```
rpm –V 已安装的包名
	-V 校验指定RPM包中的文件（verify）

验证内容中的8个信息:
- S 文件大小是否改变
- M 文件的类型或文件的权限（rwx）是否被改变
- 5 文件MD5校验和是否改变（可以看成文件内容是否改变）
- D 设备的中，从代码是否改变
- L 文件路径是否改变
- U 文件的属主（所有者）是否改变
- G 文件的属组是否改变
- T 文件的修改时间是否改变

文件类型:
- c 配置文件（config file）
- d 普通文档（documentation）
- g “鬼”文件（ghost file），很少见，就是该文件不应该被这个RPM包包含
- l 授权文件（license file）
- r 描述文件（read me）
```

# 从RPM包中提取文件
- 当某些文件损坏或或者丢失
- 从RPM包中提取文件，放到合适的地方

```
rpm2cpio 包全名 | cpio -idv 文件路径
- rpm2cpio #将rpm包转换为cpio格式的命令
- cpio #是一个标准工具，它用于创建软件档案文件和从档案文件中提取文件

cpio 选项 < [文件|设备]
	-i： copy-in模式，还原
	-d：还原时自动新建目录
	-v：显示还原过程

[root@localhost ~]# rpm -qf /bin/ls   #查询ls命令属于哪个软件包
[root@localhost ~]# mv /bin/ls /tmp/  #造成ls命令误删除假象

[root@localhost ~]# rpm ‐qf /bin/ls  #查询文件所属的软件包
#提取RPM包中ls命令到当前目录的/bin/ls下
[root@localhost ~]# rpm2cpio /mnt/cdrom/Packages/coreutils-8.4-19.el6.i686.rpm | cpio -idv ./bin/ls
#把ls命令复制会/bin/目录，修复文件丢失
[root@localhost ~]# cp /root/bin/ls /bin/

```