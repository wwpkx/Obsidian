```
/etc/profile
/etc/profile.d/*.sh
~/.bash_profile
~/.bashrc
/etc/bashrc
```
![](../photo/Pasted%20image%2020230522144434.png)

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
