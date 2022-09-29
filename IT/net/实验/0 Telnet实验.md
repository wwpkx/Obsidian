![](../../photo/Pasted%20image%2020220929190152.png)

```
user-interface vty 0 4
  authentication-mode aaa
  user privilege level 15
aaa 
  local-user admin password cipher huawei
  local-user admin privilege level 15
  local-user admin service-type telnet

```