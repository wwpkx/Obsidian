# 环境变量配置文件
![](../photo/Pasted%20image%2020230522144434.png)

```
/etc/profile
/etc/profile.d/*.sh
~/.bash_profile
~/.bashrc
/etc/bashrc

/etc/profile的作用：
- USER变量：
- LOGNAME变量：
- MAIL变量：
- PATH变量：
- HOSTNAME变量：
- HISTSIZE变量：
- umask：
- 调用/etc/profile.d/*.sh文件

~/.bash_profile的作用
- 调用了~/.bashrc文件。
- 在PATH变量后面加入了“:$HOME/bin”这个目录

~/.bashrc的作用
- 定义默认别名
- 调用/etc/bashrc

/etc/bashrc的作用
- PS1变量
- umask
- PATH变量
- 调用/etc/profile.d/*.sh文件
```
# ~/.bash_logout
```
1、注销时生效的环境变量配置文件
~/.bash_logout

2、 其他配置文件
~/bash_history

3、 Shell登录信息
  1）本地终端欢迎信息： /etc/issue
	转义符 作 用
	\d 显示当前系统日期
	\s 显示操作系统名称
	\l 显示登录的终端号， 这个比较常用。
	\m 显示硬件体系结构， 如i386、 i686等
	\n 显示主机名
	\o 显示域名
	\r 显示内核版本
	\t 显示当前系统时间
	\u 显示当前登录用户的序列号

  2）远程终端欢迎信息： /etc/issue.net
	- 转义符在/etc/issue.net文件中不能使用
	- 是否显示此欢迎信息，由ssh的配置文件/etc/ssh/sshd_config决定，加入“Banner/etc/issue.net”行才能显示（记得重启SSH服务）

  3）登陆后欢迎信息： /etc/motd
	不管是本地登录，还是远程登录，都可以显示此欢迎信息
```
# 设置环境变量
```
export 变量名=变量值  
#申明变量  

env  
#查询变量  

unset 变量名  
#删除变量


例子 PATH
PS1：定义系统提示符的变量
PATH="$PATH":/root/sh

例子 PS1
PS1：定义系统提示符的变量
\d：显示日期，格式为“星期 月 日”
\h：显示简写主机名。如默认主机名“localhost”
\t：显示24小时制时间，格式为“HH:MM:SS”
\T：显示12小时制时间，格式为“HH:MM:SS”
\A：显示24小时制时间，格式为“HH:MM”
\u：显示当前用户名
\w：显示当前所在目录的完整名称
\W：显示当前所在目录的最后一个目录
\#：执行的第几个命令
\$：提示符。如果是root用户会显示提示符为“#”，如果是普通用户会显示提示符为“$”

举例：
[root@localhost ~]# PS1='[\u@\t \w]\$ '
[root@04:50:08 /usr/local/src]#PS1='[\u@\@ \h \# \W]\$‘
[root@04:53 上午 localhost 31 src]#PS1='[\u@\h \W]\$ '
```
