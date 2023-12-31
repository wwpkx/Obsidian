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

```

1. 下载 Ubuntu Desktop 容器
docker pull kasmweb/ubuntu-jammy-desktop:1.14.0 

2. 创建并运行 Ubuntu Desktop 容器
docker run -d --restart=unless-stopped \
--name ubuntu-desktop \
-p 6901:6901 \
-e VNC_PW=password \
-e LANG=zh_CN.UTF-8 \
-e LANGUAGE=zh_CN:zh \
-e LC_ALL=zh_CN.UTF-8 \
-v /home/docker/ubuntu-desktop/shares:/home/kasm-user/shares \
--shm-size=512m \
kasmweb/ubuntu-jammy-desktop:1.14.0

3. 运行以下命令设置 root 密码
docker exec -u root -it ubuntu-desktop passwd

4. 停止/启动/重启/删除 ubuntu desktop 容器
docker stop/start/restart/rm ubuntu-desktop

使用 Ubuntu Desktop
1. 浏览器访问 https://服务器 ip:6901 (如果显示不正常，请按住 ctrl 后刷新页面)
用户名：kasm_user
密码：password  (VNC_PW 的值)

2. 剪贴板共享
使用 chrome/edge 浏览器时，支持剪贴板共享。

3. 文件共享
在 Ubuntu Desktop 里，将文件保存在/home/kasm-user/shares 目录下，会永久存放主机的/home/docker/ubuntu-desktop/shares 目录下，通过这个目录可实现文件共享。

4. root 登录
在 Ubuntu Desktop 里的终端，使用以下命令以 root 登录后可以管理系统、安装软件。
su root --login

5. 中文输入法
在网页上的 KasmVNC 的设置中，启用输入法 (IME)，即可使用浏览器所属操作系统的中文输入法 (不是 ubuntu 里的输入法)
macos 切换到中文输入法，即可在 Ubuntu Desktop 输入中文了
windows 可能需要以下设置才能使用： 在“设置->时间和语言->语言->首选语言”添加“英语(美国)” 在首次使用时，将系统输入法切换成美式键盘，再切换成中文才能使用
目前还没搞定声音，按官方介绍需要安装 Kasm Workspaces ，如果有 V 友搞定了，可以介绍一下方法。

```