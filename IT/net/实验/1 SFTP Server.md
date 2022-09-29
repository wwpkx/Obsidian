![](../../photo/Pasted%20image%2020220929191036.png)
```
[Huawei] display rsa local-key-pair public
[Huawei] rsa local-key-pair create
[Huawei] user-interface vty 0 4
[Huawei-ui-vty0-4] authentication-mode aaa
[Huawei-ui-vty0-4]protocol inbound ssh
[Huawei] aaa
[Huawei-aaa] local-user AR password cipher huawei
[Huawei-aaa] local-user AR ftp-directory flash:/
[Huawei-aaa] local-user AR service-type ssh 
[Huawei-aaa] local-user AR privilege level 15
[Huawei]ssh user AR authentication-type password
[Huawei]sftp server enable 

```