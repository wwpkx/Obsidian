# 开启Hyper-V服务
- 启用windows Hyper-V虚拟引擎
- 启用或关闭 windows 功能
- Hyper-V平台

# wsl
- 查看是否开启Linux子系统
- 启用或关闭 windows 功能
- 查看是否开启Linux子系统

# 配置镜像加速
- [阿里容器镜像服务 (aliyun.com)](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors?accounttraceid=c6b27c906c5548d6b9fee2bfa1c6862afvxh)
- `C:\Users\Administrator\.docker\daemon.json

```
{
  "builder": {
    "features": {
      "buildkit": true
    },
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,

# 配置镜像加速
  "registry-mirrors": [
    "https://48vg54vg.mirror.aliyuncs.com",
  	"https://hub-mirror.c.163.com",
  	"https://mirror.baidubce.com"
  ]
}

```

# 更改默认镜像地址
```
# 查看镜像
# Docker Desktop 通过WSL2启动，会自动创建2个子系统，分别对应2个 vhdx 硬盘映像文件[docker-desktop-data与docker-desktop]
wsl --list -v 
wsl --shutdown

# 导出映像文件
wsl --export docker-desktop F:\docker-vm-source\distro\docker-desktop.tar
wsl --export docker-desktop-data F:\docker-vm-source\data\docker-desktop-data.tar  

# 注销原来的docker
wsl --unregister docker-desktop-data
wsl --unregister docker-desktop

# 从tar 文件，将导出的 Docker 镜像再导入回wsl，并设置挂载目录
# wsl --import <Distribution Name> <InstallLocation> <FileName>
wsl --import docker-desktop-data F:\docker-vm-source\data\  F:\docker-vm-source\data\docker-desktop-data.tar --version 2
wsl --import docker-desktop F:\docker-vm-source\distro\  F:\docker-vm-source\distro\docker-desktop.tar --version 2
```