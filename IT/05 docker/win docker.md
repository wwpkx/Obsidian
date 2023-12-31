- 下载docker desktop
- 开启hypter-v服务
- 开启wsl
- 配置镜像加速
- 更改默认镜像位置
# 开启Hyper-V服务
- 启用windows Hyper-V虚拟引擎
- 启用或关闭 windows 功能，Hyper-V平台
# 开启wsl
- 查看是否开启Linux子系统
- 启用或关闭 windows 功能，查看是否开启Linux子系统
- `wsl --update`

# 配置镜像加速
- [阿里容器镜像服务 (aliyun.com)](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors?accounttraceid=c6b27c906c5548d6b9fee2bfa1c6862afvxh)
- `C:\Users\Administrator\.docker\daemon.json

```
{
    "builder":{
        "gc":{
            "defaultKeepStorage":"20GB",
            "enabled":true
        }
    },
    "experimental":false,
    "registry-mirrors":[
        "https://registry.docker-cn.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://cr.console.aliyun.com",
        "https://mirror.ccs.tencentyun.com",
        "https://hub-mirror.c.163.com",
        "https://mirror.baidubce.com"
    ]
}
```

# 更改默认镜像地址
## 通过wsl更改（更方便）
- WSL发行版默认安装路径在 `%LOCALAPPDATA%/Docker/wsl` 目录
- 步骤
	- 关闭Docker Desktop
	- 在命令行输入关闭wsl的命令：`wsl --shutdown`
	- 将docker-desktop-data导出到你想放置的位置
	- 
```
# 查看镜像
# Docker Desktop 通过WSL2启动，会自动创建2个子系统，分别对应2个 vhdx 硬盘映像文件
# - docker-desktop-data
# - docker-desktop
wsl --list -v 
wsl --shutdown

# 导出映像文件
wsl --export docker-desktop F:\docker\wsl\distro\docker-desktop.tar
wsl --export docker-desktop-data F:\docker\wsl\data\docker-desktop-data.tar  

# 注销原来的docker
wsl --unregister docker-desktop-data
wsl --unregister docker-desktop

# 从tar 文件，将导出的 Docker 镜像再导入回wsl，并设置挂载目录
# wsl --import <Distribution Name> <InstallLocation> <FileName>
wsl --import docker-desktop-data F:\docker\wsl\data\  F:\docker\wsl\data\docker-desktop-data.tar --version 2
wsl --import docker-desktop F:\docker\wsl\distro\  F:\docker\wsl\distro\docker-desktop.tar --version 2
```

## 通过设置方式修改
- Settings -> Resources -> Advanced -> Disk image location
