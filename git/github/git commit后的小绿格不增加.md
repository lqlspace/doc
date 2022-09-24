### 原因

github配置的账号跟本地commit账户不一致

### 修改

1. github添加账号：

<img src="../../images/2022-09-24-19-12-36-image.png" title="" alt="" width="480">

2. 登录本地repo目录，执行如下命令

```
git config --local user.name "username"
git config --local user.email "username@xiaoheiban.com"
```
