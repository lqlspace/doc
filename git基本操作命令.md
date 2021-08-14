## 1. git的数据流走向
![](./media/git-data-transport-commands.png)

## 1. 账号配置：
- 配置全局账号
```cassandraql
git config --global user.name "username"
git config --global user.email "username@xiaoheiban.com"
```
- 配置项目级账号
> 须进入项目目录下配置（应用场景：github和gitlab使用不同账号）
```cassandraql
git config --local user.name "username"
git config --local user.email "username@xiaoheiban.com"
```
- 删除某项配置
```
git config [--local|--global] --unset user.name 
git config [--local|--global] --unset user.email
```
- 查看配置
```
git config --list 
```

- 配置rebase方式拉取远程代码&rebase是字段autostash
```cassandraql
git config pull.rebase true
git config rebase.autoStash true

//平时pull是最后执行如下命令
git pull --rebase --autostash (zsh: gupa)
```

## 2. pick其他分支的commit至当前分支
- pick单次提交
```cassandraql
git cherry-pick b45b271b32    // hash值为要pick的commit的hash
```
- pick连续多个提交
```cassandraql
git cherry-pick b45b271b32^..e674a524a8  // 闭区间，前面的先提交的
```

## 3. 合并分支
将develop合并到master：

```
(1) git pull --rebase (先将本地的分支更新至最新，防止其他人提交到此分支的commit丢失)
(2) git fetch -a
(3) git rebase origin/master
(4) 如果出现冲突，解决方案如下：
    a、采纳本分支commit的代码，或重新修改的代码，解决冲突后，执行如下语句：
    step1:  git add . （注：不要执行git commit) 
    step2:  git rebase --continue
    b、如果采用head的代码，说明本次修改的commit无效，则直接跳过本次commit:
        git rebase --skip
    c、若感觉rebase过程已混乱，无法继续rebas，则可执行如下操作退出本次rebase，代码还原成git rebase origin/master前的状态
        git rebase --abort
(5) rebase过程结束后，push到远程分支
    git push -f (注：此为强推，如果此间有人提交代码至远程分支，会被覆盖)
```

## 13. 清除修改
- 清除工作区的修改：
```cassandraql
git checkout -- .
```
- 清除stage区的修改：
```cassandraql
git reset HEAD filename
```
- 清除本地已提交，但未push到远程的最近一次修改：
```cassandraql
git reset HEAD^   //即回退到上个版本
```
- 回退至指定版本：
```cassandraql
git reset 62091c90  //hash为要回退到的commit的hash
```
- 使用rebase回退众多提交中的某次提交
```cassandraql
1、找到想要删除的commit的前一次提交的commit id
2、git rebase -i xxxx (前次commit id)
3、将要删除的commit由pick改为drop 
4、保存并push
```
- 使用revert回退众多提交中的某次提交
```cassandraql
git revert a11bdeadaa  //即产生一个新的revert commit，该commit回退了hash对应提交的代码
```
- 回退push到远程的commit
```cassandraql
将本地的修改直接push到远程即可
```

## git revert和git reset的区别
revert：放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在；  
reset：将HEAD指针指到指定提交，log中不会出现放弃的提交记录。

## git reset --mix（默认）、--soft、--hard的区别
--mix：所有丢弃的commit的修改变为工作区的修改，如下操作后仍可提交
```cassandraql
git add .
git commit 
```
--soft: 所有丢弃的commit的修改变为stage区的修改，如下操作仍可提交
```cassandraql
git commit 
```
--hard：所有丢弃的commit的修改，包括本地为提交的修改均会被remove，慎用！！！
## 4. 恢复工作区误删除的文件
```
git ls-files -d | xargs git checkout --
```


## 5. 删除分支
- 删除本地分支：git branch -d(-D) feature_test
- 删除远程分支：git push origin :feature_test
- 清除远程在本地存在，而远程已经删除的分支：git remote prune origin

## 6. 恢复工作区文件 & 切换已有分支
- 恢复工作区文件
```cassandraql
// 恢复单个文件
git checkout -- file

// 恢复当前目录下所有文件
git checkout -- .
```
- 切换已有分支
```cassandraql
git checkout hera   // 若报错，加--track选项
```

## 7. hunk提交
> 自己筛选该提交哪些代码块  
```cassandraql
git add --path(或者-p)  (zsh: gapa)

//交互帮助：
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
J - leave this hunk undecided, see next hunk
e - manually edit the current hunk
? - print help
```

## 8. 查看提交log
- 查看单行提交记录 
```cassandraql
glog/glod : git log --oneline --decorate --graph
```
- 查看某个文件的更改
```cassandraql
//切换到某文件所在目录，然后执行：
glod/glog filename

// 同时查看修改：
glgp filename 

// 只看某次提交的某个文件变化：
git show c5e69804bbd9725b5dece57f8cbece4a96b9f80b filename
```

## 9. git stash
- 查看stash列表
```cassandraql
git stash list 
```
- stash时自定义说明
```cassandraql
git stash save "xxx"
```
- stash时同时把untrack的文件一起stash，并自定义说明
```cassandraql
git stash save [-u|-a] "xxx"
```
- 弹出顶层stash记录
```cassandraql
git stash pop
```
- 弹出特定stash记录,
```cassandraql
//step1: 查看想要弹出的记录的最左侧
git stash list 
//step2: 还原特定stash 
git stash apply stash@{x}
```

## 10. 提交远程分支
- 首次提交分支，需要加-u与远程分支关联： git push -u origin develop /(gpsup)
- 关联过只用：git push /(gp)


## 11. 拉分支
- 更新当前分支：git pull
- 删除远程已被删除的分支： git remote prune origin

## 12. 查看远程地址
- 查看远程仓库地址（即：git clone的地址）
```cassandraql
git remote -v 
```

- **查看命令history**
```
git  reflog
注意：该命令常用于重返未来的某个版本（我们回退到以前版本后，未来版本的commitid号有可能找不着）
```

- 删除版本库中文件  
`git  rm  filename`

## 使用ssh密钥通信
- 使用ssh-keygen命令生成一对公私钥
``
- 添加公钥至远程仓库


## 新建远程空仓库  
- github上创建空仓库learngit  
```
在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库。
目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
```

- 关联一个远程库  
`git remote add origin git@server-name:path/repo-name.git`  
```
请千万注意，把上面的michaelliao替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
```


- 关联后，第一次推送master分支所有内容  
｀git push -u origin master`  
- 此后，每次本地提交后，只要有必要，就可以使用命令  
`git push origin master推送最新修改`

## 从远程仓库克隆
git clone *.git

## 分支 
查看分支：git branch  

查看本地&远程所有分支 git branch -a

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除本地分支：git branch -d <name>

删除远程分支： git push origin --delete <name>

## 冲突
```
用git log --graph命令可以看到分支合并图。
```

## 保护现场，修复bug
```
1、git stash将现在的context隐藏；
2、git status查看是否已清空，清空则可继续下一步；
3、确定需要在哪个分支进行bug修复，然后git checkout 到指定分支，比git checkout master基于主线进行bug修复；
4、git checkout -b bug-01基于主线切换出bug分支进行修复；
5、git add & git commit提交
6、git checkout master切换会主分支；
7、git merge bug-01将bug分支合并到当前主分支；
8、git branch -b bug-01删除bug分支；
9、git checkout dev，切换回原来分支；
10、git stash list查看之前隐藏的记录；
11、git stash pop 弹出隐藏现场（相当于git stash apply & git stash drop），之后就可以继续工作了
```

# 远程分支，多人协作
- 查看远程分支  
```
git remote
git remote -v

```
- 切换至其他分支  
```
git clone最初只有master分支，想切换到其他分支可采取如下方式：
1、直接git checkout dev切换（即创建本地dev分支并与远程dev分支关联）
2、git checkout -b localname origin/remotename //此处本地分支名与远程分支名最好保持一致，如果不一致git pull和push时并没有关联起来；
3、git branch --set-upstream-to=origin/dev  dev关联本地dev与远程分支dev分支（没有关联时，常显示no tracking information错误）
4、此时git pull即可默认拉取远程origin/dev的更新了

```
- 推送分支
```
git push origin master //将本地master分支推送到远程origin仓库
git push origin dev  //将本地dev分支推送到远程origin仓库
```
- 抓去分支  
```
git pull
```


# 标签
- 创建标签
```
git  tag  <name>  //给本分支最新提交打标签
git  tag  <name>  <commitid>  //给某次提交打标签
git tag -a v0.1 -m "version 0.1 released" 1094adb  //-a 指定标签名，-m 指定注释
```
- 查看标签
```
git tag    //查看所有标签
git tag  <tagname>   //查看某标签详细信息
```

- 删除标签
```
1、删除本地标签
git log -d v0.1

2、删除远程标签
（1）git log -d v0.1  //先从本地删除
（2）git push origin :refs/tags/v0.1  //然后从远程删除

```
- 推送某个标签到远程
```
git push origin v0.1  
git push origin --tags  //一次性推送全部尚未推送至远程的tag
```


# 不小心清空了Stash，如何恢复
```
1、如果知道drop的commitID，可直接如下恢复：
git stash apply $commitid

2、可以通过如下形式找到drop的commitid号
git fsck --no-reflog | awk '/dangling commit/ {print $3}'
```

## 查看单个文件的修改记录
```
git  log  -p  filename
```

## 切换分支时，现有分支的更改同步更改至切换后的分支


## 查看git上个人代码量
```
git log --author="username" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -

//git log的$1, $2为单个文件的add、sub行数
```

## 统计每个人增删行数
```
git log --format='%aN' | sort -u | while read name; do echo -en "$name\t"; git log --author="$name" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -; done
```






### GIT练习
[GIT 练习](https://learngitbranching.js.org/?locale=zh_CN)
