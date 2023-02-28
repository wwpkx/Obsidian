# 插件
- EditConfig
	- 大家使用同一份配置，比如空格/tab，文件编码等
- Project Manager 
	- 管理项目，类似常用的ide工具

# 工程配置
## tasks.json
- 配置可以执行的tasks
```
{
    "version": "2.0.0", 
    "tasks": [
        {
            "label": "写映像文件",
            "type": "shell",
            "command": "bash ${workspaceRoot}/script/img-write-osx.sh",
        },
        {
            "label": "启动qemu",
            "type": "shell",
            "command": "bash ${workspaceRoot}/script/qemu-debug-osx.sh",
        },
        {
            "label": "调试准备",
            "dependsOrder": "sequence",
            "dependsOn": [
                "写映像文件",
                "启动qemu"
            ],
        }
    ]
}
```