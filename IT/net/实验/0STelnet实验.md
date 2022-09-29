![](../../photo/Pasted%20image%2020220929190225.png)
```
rsa local-key-pair create
display rsa local-key-pair public
user-interface vty 0 4
   authentication-mode aaa
   protocol inbound ssh
aaa
   local-user admin privilege level 3
   local-user admin service-type ssh
ssh user admin authentication-type password
stelnet server enable
display ssh user-information
display ssh server status
display ssh server session


ssh client first-time enable
stelnet 10.1.1.1

```