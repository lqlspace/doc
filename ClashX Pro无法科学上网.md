### 原因 
[参考链接](https://sobaigu.com/clash-timeout-failed-by-dns.html)
- 本机时间跟网络时间存在时差
```cassandraql
系统设置时间跟网络同步即可 
```
- 内置DNS为true
```cassandraql
yml 配置文件，将 dns 下的 enable 值改为 false；
```
