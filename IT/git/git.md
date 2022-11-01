# .gitignore无法忽略某些文件
```
.gitignore忽略文件只能忽略那些还没有纳入版本控制的文件
如果文件已经被纳入了版本控制中，则修改.gitignore将不能生效

需要先删除掉本地缓存库后提交  
git rm -r –cached .idae
```


