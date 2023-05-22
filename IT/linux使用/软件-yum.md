# yum源
```
[root@localhost yum.repos.d]# vi /etc/yum.repos.d/CentOS-Base.repo
- [base] 容器名称，一定要放在[]中
- name 容器说明，可以自己随便写
- mirrorlist 镜像站点，这个可以注释掉
- baseurl 我们的yum源服务器的地址。默认是CentOS官方的yum源服务器
- enabled 此容器是否生效，如果不写或写成enable=1都是生效
- gpgcheck 如果是1是指RPM的数字证书生效，如果是0则不生效
- gpgkey 数字证书的公钥文件保存位置。不用修改
```
# 常用yum命令
```
1）查询
[root@localhost yum.repos.d]# yum list
#查询所有可用软件包列表

[root@localhost yum.repos.d]# yum search 关键字
#搜索服务器上所有和关键字相关的包

2）安装
[root@localhost yum.repos.d]# yum –y install 包名
选项：
	install 安装
	-y 自动回答yes

3）升级
[root@localhost yum.repos.d]# yum -y update 包名
选项：
	update 升级
	-y 自动回答yes

4）卸载
[root@localhost yum.repos.d]# yum -y remove 包名
选项：
	remove 卸载
	-y 自动回答yes
```
