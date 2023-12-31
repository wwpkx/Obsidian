```
#run
docker run -d --shm-size=512m -p 1201:6901 --name kasmsudo -e VNC_PW=dddd kasmweb/desktop:1.14.0-rolling
 
#浏览器访问
https://localhost:1201
 
#账号密码.密码是容器创建时设置的。
#该账号通过sudo命令可以使用管理员功能。
kasm_user
dddd 
```