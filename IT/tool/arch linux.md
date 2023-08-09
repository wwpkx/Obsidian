# 更新源
```
# 一、修改源
vim /etc/pacman.d/blackarch-mirrorlist

##清华
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxarm/$arch/$repo
##中科大
Server = http://mirrors.ustc.edu.cn/archlinuxarm/$arch/$repo
##成都电子科大
Server = http://mirrors.stuhome.net/archlinuxarm/$arch/$repo


# 二、更新密钥
# 如果你这样更新完毕之后，在安装新软件会遇到“导入密钥”类似报错的问题。
# 这是由于pacman的GPG校验密钥改变所造成的。你需要运行如下命令来更新密钥
# 使用方法一
[ blackarch ~ ]# sudo pacman-key --init 
[ blackarch ~ ]# sudo pacman-key --populate archlinuxarm

# 或者使用方法二（推荐）
[ blackarch ~ ]# sudo pacman -S archlinux-keyring


# 三、
[ blackarch ~ ]# pacman -Syu  #同步源并更新系统
```