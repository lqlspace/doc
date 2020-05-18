## 项目分支介绍

- 主分支`master`

- 预生产分支`pre`

- 测试分支`test`

- 功能分支`feature/版本号`

- 修复分支`hotfix/xxx`

## git 开发流程

无论是开发新功能还是修复线上 bug 都需遵循以下步骤及规范

1. 从`master`切一个新分支

   ```shell
   // 切到master分支
   git checkout master or gcm or gcm

   // 拉取最新代码
   git pull --rebase --autostash or  gupa

   // 切一个新的分支，功能分支起名「feature/版本号」，修复分支起名「hotfix/版本号」
   git checkout -b feature/111 or gcb feature/111
   ```

2. `commit` 提交规范
![规范链接](https://www.conventionalcommits.org/en/v1.0.0/)

   ```shell
   #完成了怎么样的事情
   git commit -m feat: xx
   #12345(bugId)什么样子的bug
   git commit -m fixed: xx
   #关于什么的重构
   git commit -m refactor: xx
   #不会修改src或测试文件的更改。例如更新构建任务，程序包管理器
   git commit -m chore: xxx
   #代码样式，不影响代码含义的更改（空白，格式，缺少分号等）
   git commit -m style: xx
   #添加缺少的测试或更正现有测试
   git commit -m test: xxx
   #还原提交
   git commit -m revert: xx
   ```

3. 多人开发同个功能分支时

   ```shell
   // 当远程的提交多于本地提交时，一定要
   git pull --rebase --autostash or gupa

   git push or gp
   ```

4. 功能开发完毕，提测

   ```
   // 切到test分支
   git checkout test or gco test

   // 拉取test代码
   git pull --rebase --autostash or gupa

   // 合并功能分支
   git merge --no-ff feature/111 or gm --no-ff feature/111

   // push到远程
   git push or gp
   ```

5. 发布预发环境

   发布过程同 `步骤3`，分支改为 `pre`

6. 测试通过后，需要 rebase 最新的 `master` 分支

   ```
   // 切到master分支
   git checkout master or gcm

   // 拉取最新master代码
   git pull --rebase --autostash or gupa

   // 再切到当前开发的分支
   git checkout feature/xxx or gco feature/xxx

   // rebase master
   git rebase master or grbm

   // 如果rebase 产生冲突，手动解决冲突，然后
   git add . or ga .
   git rebase --continue or grbc

   // rebase 完之后
   git push or gp

   // 注意：当 rebase 过程中需要解决冲突
   // 在 rebase 完成之后
   // git push 无效后 需要用下面的命令
   // !!! 用这个命令前 一定要确保本地代码跟线上代码的同步
   // git pull --rebase 再一次
   git push --force-with-lease or gpf
   ```

7. 在 gitlab 上提一个 MR (merge request)，让负责人合并代码

> 更加详细的 git 操作

- [Git 基本流程步骤](Git 基本流程步骤.md)
- [Git 常用命令](Git常用命令.md)
- [Git 飞行规则](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-CN.md)

# 4. 开发流程（第二天讲解）

## 项目流程

1. 项目评审

   会议形式（产品、研发、UI、测试）

2. 项目估时

   各自相关开发估时（包括开发过主流程用例时间<根据项目而定>）

3. 进入开发

   按照估时日期完成（意外因素除外）

   开发跑主流程（保证主流程自测通过）

4. 进入测试

   test 环境

   pre 环境

5. 上线

   测试完成上线（确保线上环境正常）
