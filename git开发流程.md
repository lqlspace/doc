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
   git checkout master（gcm）

   // 拉取最新代码
   git pull --rebase --autostash (gupa)

   // 切一个新的分支，功能分支起名「feature/版本号」，修复分支起名「hotfix/版本号」
   git checkout -b feature/111 or gcb feature/111
   ```

2. `commit` 提交规范  
> [规范链接](https://www.conventionalcommits.org/en/v1.0.0/)

   ```shell
type: commit 的类型
feat: 新特性
fix: 修改问题
refactor: 代码重构
docs: 文档修改
style: 代码格式修改, 注意不是 css 修改
test: 测试用例修改
chore: 其他修改, 比如构建流程, 依赖管理.
scope: commit 影响的范围, 比如: route, component, utils, build...
subject: commit 的概述, 建议符合  50/72 formatting
body: commit 具体修改内容, 可以分为多行, 建议符合 50/72 formatting
footer: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.

作者：阿里南京技术专刊
链接：https://juejin.im/post/5afc5242f265da0b7f44bee4
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
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
