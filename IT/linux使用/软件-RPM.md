- Redhat Package Manager（Redhat软件包管理工具）
- apache-1.3.23-11.i386.rpm
	- “apache”：软件名称
	- “1.3.23-11”：软件的版本号，主版本和次版本
	- “i386”：是软件所运行的硬件平台
	- “rpm”：文件扩展名，代表RPM包

# RPM包依赖性
- 树形依赖： abc
- 环形依赖： abca
- 模块依赖： 模块依赖查询网站：
	- www.rpmfind.net

# 命令
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