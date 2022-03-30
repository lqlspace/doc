### 使用ClashX科学上网的话，可以设置通过代理下载
- 1. 修改git配置
```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```
注：此处1080端口根据代理客户端实际配置接口而定；

- 2. 直接修改.bash_profile或.zshrc文件，对git外的其他应用层命令也生效；
```
# 设置使用代理
alias setproxy="export http_proxy=http://127.0.0.1:7890; export https_proxy=$http_pro
xy; export all_proxy=socks5://127.0.0.1:7890; echo 'Set proxy successfully'"
# # 设置取消使用代理
alias unsetproxy="unset http_proxy; unset https_proxy; unset all_proxy; echo 'Unset p
roxy successfully'"
```
注：此处http_proxy和https_proxy均设定的http协议，https_proxy协议设置成https，反而报如下错误（跟git相反）：
```
❯ curl https://www.google.com
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:7890
```