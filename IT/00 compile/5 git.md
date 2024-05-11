![](../photo/Pasted%20image%2020221101174211.png)

# .gitignore无法忽略某些文件
```
.gitignore忽略文件只能忽略那些还没有纳入版本控制的文件
如果文件已经被纳入了版本控制中，则修改.gitignore将不能生效

需要先删除掉本地缓存库后提交  
git rm -r –cached .idae
```

# 分支
```
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>或者git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
```

# git pull和git fetch
![](../photo/Pasted%20image%2020221101174032.png)
```
git remote -v
git push origin branch-name //从本地推送分支
git branch --set-upstream branch-name origin/branch-name //建立本地分支和远程分支的关联
git branch --set-upstream-to=origin/dev dev

git fetch <远程主机名> <远程分支名>:<本地分支名--不存在的分支>
git fetch origin master:temp 
1. 从远程的 origin仓库 的 master分支 下载代码到本地, 并新建一个 temp分支
2. 没有冒号，则表示将远程origin仓库 的master分支 拉取下来到 本地当前分支

git pull <远程主机名> <远程分支名>:<本地分支名>
git pull origin master:branchtest
1. 将 远程主机origin 的master分支 拉取过来，与 本地的branchtest分支合并
2. 没有冒号，则表示将 远程origin仓库的 master分支 拉取下来与 本地当前分支合并
```

# git 无法保持密码
```
在文件 C:\Users\Administrator\.gitconfig 中，添加 [credential]

[user]
	name = wwpkx
	email = wwpkx@163.com
[credential]
	helper = store
```

