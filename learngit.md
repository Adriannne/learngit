Git是目前世界上最先进的分布式版本控制系统（没有之一）
github基础：http://www.runoob.com/w3cnote/git-guide.html

### 1.git安装
```
# 安装:
sudo apt-get install git

# 查看:
git && git --help
git --version

# 配置:
git config --list

git config --global user.name "runoob"
git config --global user.email test@runoob.com
```

### 2.git版本库
```
git init    #这个目录变成Git可以管理的仓库
git init <dir>   #选择一个已经有的目录下创建Git仓库

git clone <repo>  #克隆远程仓库到本地
```
远程仓库
https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416

1.在/home/user/.ssh/查看自己的ssh key
2.如果没有, 创建ssh key:
```
ssh-keygen -t rsa -C "youremail@example.com"
cat /home/user/.ssh/id_rsa.pub
```
3.登陆GitHub，打开“Account settings”，“SSH Keys”页面添加key

### 3.查看状态

ps: git只能跟踪文本文件修改, 不能跟踪二进制文件.

不要用windows自带编辑器写任何东西.

```
git status         #查看仓库当前状态

# 查看改动的区别：
git diff           #查看未缓存的改动
git diff --cached  #查看已缓存的改动
git diff --stat    #查看改动摘要
git diff HEAD      #查看所有改动

# 查看提交历史：
git log
git log --pretty=oneline   #在一行显示
```

### 4.提交文件
```
git add <file>  #添加文件
git commit -m "<comment>"  #提交文件
git push    #推送到远程仓库
```

### 5.删除文件
```
rm <file>
git rm <file>
git rm -f <file>   #强制删除
git rm -r <dir>    #递归删除

git commit -m "remove file"
```

### 6.分支管理
```
git branch          #查看本地分支
git branch -a       #查看远程分支号

git branch <dev>      #创建分支

git checkout <dev>    #切换到分支,用于切换分支或恢复工作树文件

git merge master    #将master合并到当前branch

git commit -m "new branch"   #提交修改

git branch -d <dev>   #删除分支

git log --graph  #查看分支合并图

git merge --no-ff -m "merge with no-ff" dev    #禁用fastforward模式
```
fastforward:当前分支合并到另一分支时，如果没有分歧解决，就会直接移动文件指针。这个过程叫做fastforward。

举例来说，开发一直在master分支进行，但忽然有一个新的想法，于是新建了一个develop的分支，并在其上进行一系列提交，完成时，回到 master分支，此时，master分支在创建develop分支之后并未产生任何新的commit。此时的合并就叫fast forward。

### 7.缓存区
工作区：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区。

版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
    
stage（index）暂存区，git add将需要提交的文件修改通通放到暂存区，然后，git commit一次性提交暂存区的所有修改。
    
```
git stash    #将未提交修改放入暂存区
git stash list    #查看暂存区
git stash pop stash@{n}    #恢复到工作区
```

### 8.版本回退

具体参考：http://www.runoob.com/git/git-workspace-index-repo.html

版本回退，连同log一起更改.

HEAD当前版本，HEAD^ 上一个版本，HEAD^^ 上上个版本
```
#版本回退
git reset --hard HEAD^
git reset --hard <commit id>

git reset HEAD    #用master替换缓存区内容

git checkout -- <file>   #丢弃工作区的修改，会用暂存区文件替换工作区文件
git checkout HEAD <file> #用master替换缓存区和工作区内容

git rm --cached <file>   #从暂存区删除文件
```

如果版本回退后想查看新版本, 可通过git reflog找到新的commit点, 然后git reset切回去.
```
(base) user@conti-del-lp-039:~$ git reflog
aac8f1d HEAD@{0}: reset: moving to HEAD^
f294dce HEAD@{1}: commit: update run_loc_with_json_master.py
aac8f1d HEAD@{2}: commit: update pull_code.sh according to new rdb40 localization repo
638827f HEAD@{3}: checkout: moving from master to LOC
f81f22a HEAD@{4}: clone: from ssh://git@stash.ygomi.com:7999/r_poc/rdb_test_integration_script.git

(base) user@conti-del-lp-039:~$ git reset f294dce
```

### 9.撤销修改
```
#撤销本地文件修改
git checkout --file

#撤销已add文件修改
git reset HEAD file
git checkout --file

#撤销已commit文件修改
git reset --hard HEAD^

#撤销已push文件
rm file
git rm file
```


### 10.标签管理
```
git tag   #查看所有tag
git show <tagname>    #查看某个tag

git tag -a <tag_name>  #加tag
git tag -d <tag_name>  #删除tag

git push origin <tagname>    #推送到远程仓库
git push origin --tags    #全部推送
```

### 11.常见报错

#### error:1400410B:SSL routines:CONNECT_CR_SRVR_HELLO:wrong version number
将https改为http协议。
git clone http://github.com/jackfrued/Python-100-Days.git
可能是公司限制，ssh协议会报错没有权限。
