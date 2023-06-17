# qemu
- make
- make start，启动qumu
- make debug，使用qemu调试
```
qemu-system-x86_64 -m 16M -boot a -fda linux-0.11\Image -hda hdc-0.11.img
qemu-system-x86_64 -m 16M -boot a -fda linux-0.11\Image -hda hdc-0.11.img -s -S
qemu-system-i386 -m 16M -boot a -fda linux-0.11\Image -hda hdc-0.11.img
qemu-system-i386 -m 16M -boot a -fda linux-0.11\Image -hda hdc-0.11.img -s -S
```
- 使用gdb进行远端调试
```
$ gdb tools/system
(gdb) set architecture i386:x86-64
(gdb) target remote :1234
```
- 参考，其中代码可以直接使用qemu和bochs调试
	- [GitHub - yuan-xy/Linux-0.11: The old Linux kernel source ver 0.11 which has been tested under modern Linux, Mac OSX and Windows.](https://github.com/yuan-xy/Linux-0.11)
	- [Linux-0.11操作系统源码调试 - chaoguo1234 - 博客园 (cnblogs.com)](https://www.cnblogs.com/chaoguo1234/p/16883932.html)
	- 
# vscode + qemu
```
{
  "version": "0.2.0",
  "configurations": [
      {
          "name": "(gdb) Launch",
          "type": "cppdbg",
          "request": "launch",
          "program": "${workspaceFolder}/tools/system",
          "miDebuggerServerAddress": "127.0.0.1:1234",
          "args": [],
          "stopAtEntry": false,
          "cwd": "${workspaceFolder}",
          "environment": [],
          "externalConsole": false,
          "MIMode": "gdb",
          "setupCommands": [ 
              { 
			   "description": "run target", 	//"description":"设置体系结构",
                  "text": "set arch i386:x86-64", //"text":"-gdb-set architecture i386:x86-64",
                  "ignoreFailures": false 
              }
          ]
      }
  ]
}
```
# vscode + qemu +remotessh
- 安装remotessh插件
- 连接到远端服务器
- 在文件中打开工程
- 调试
![](../photo/Pasted%20image%2020230411173847.png)