# 开启Hyper-V服务
- 启用windows Hyper-V虚拟引擎
- 启用或关闭 windows 功能
- Hyper-V平台

# wsl
- 查看是否开启Linux子系统
- 启用或关闭 windows 功能
- 查看是否开启Linux子系统

# 启动Docker Desktop

# 配置镜像加速
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
wsl --list -v 
wsl --shutdown

# 导出映像文件

```