### Git常用命令


#### 1. 配置自己的用户名和邮箱：

```
git config --global user.name "username"
git config --global user.email "username@xiaoheiban.com"

// 如果某个项目你需要单独配置user.name, 可以进入到相应的目录
git config --local user.name "username"
git config --local user.email "username@xiaoheiban.com"
```


#### 2. 删除某项配置

```
git config --unset alias.co
```
 
#### 3. 分支查看：
查看本地分支：（gb）git br

查看全部分支: （gba）git br -av

#### 4. 创建分支两种方式，如从master上创建develop分支

```
// 创建新的分支，但是不会立即切到新的分支上面
gb develop master
// 创建新的分支，并且立即切到新的分支上面
gcp develop master
```


#### 5. 切换分支

```
// master
gcm
// develop
gcd
// feature/xx
gco feature/xx
```


#### 6. 合并分支
将develop合并到master：

```
// merge
// rebase 
grbm (git rebase master 基于master做rebase)
// 解决完冲突后 ga . ,不要做commit 
grbc (git rebase --continue)
// 如果不需要当前的log
grbs （git rebase --skip）
// 如果需要终止当前rebase
grba （git rebase --abort）
```

#### 7.标记
- a. 打标记：git tag v0.1.0
- b. 查看标记：git tag -l
- c. 删除标记：git tag -d v0.1.0
- d. 删除远程tag：git push origin :refs/tags/v0.9.3
- e. 推送tag：git push --tags

#### 8. 误删除的文件一次性恢复，两种方式，首先切到bash命令模式下或linux下直接用：

```
git ls-fies -d | xargs git co --
git ls-files -d | xargs -i git co {}
```


#### 9. 删除分支
- 删除本地分支：gbd feature_test
- 删除远程分支：gp origin :feature_test
- 清除远程在本地存在，而远程已经删除的分支：git remote prune origin

#### 10. 恢复文件
- gco -- file
- 命令中的“--”很重要，没有“--”，就变成了“创建一个新分支”的命令

#### 11. 查看提交log
- 1. glg|glog filename
可以看到fileName相关的commit记录
- 2. glgp
可以显示每次提交的diff
- 3. 只看某次提交中的某个文件变化，可以直接加上fileName
        > git show c5e69804bbd9725b5dece57f8cbece4a96b9f80b filename


#### 12. git stash命令

```
git stash
git stash pop
git stash list
git stash clear
git stash apply
git stash apply stash@{1}
```


#### 13. 提交远程分支
- 首次提交分支，需要加-u与远程分支关联： git push -u origin develop /(gsup)
- 关联过只用：git push /(gp)


#### 14.拉分支
- 更新当前分支：git pull
- 在master上分支上更新devlop分支：git pull origin develop:develop
- 删除远程已被删除的分支： git remote prune origin

#### 15. 查看远程地址

```
git remote -v
git remote show origin
// 修改远程关联分支
git remote set-url origin newurl
```


#### 16. 清除修改
- 清除本地未提交的修改：（grhh）git reset --hard
- 清除本地已提交但是未推送的修改：git reset --hard origin/master


#### 17. git的生命周期
![](./media/git-life-cycle.jpg)


#### 18. git的数据流走向
![](./media/git-data-transport-commands.png)
