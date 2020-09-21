# 函数err返回规范
- 如果调用的sql函数，直接返回err；
- 如果调用的是自定义函数返回的错误：
```cassandraql
1、使用logx.Errorf函数打印；
2、 打印schema: [自定义函数] err: 
```

# 查找sql语句
```cassandraql
搜索关键字："sql query"
```

# 查找es语句
```cassandraql
搜索关键字："essql"
```

# 运行时错误
```cassandraql
runtime error: invalid memory address or nil pointer dereference
``` 



# log记录函数
- 实现
```cassandraql
getLogHandler 路由层中间件 
```
- 何时打印的



```cassandraql
500 - 172.16.185.164:55238 - 683.3ms
=> POST /internal/email/send HTTP/1.0
Host: demo.xiaoheiban.cn
Connection: close
Accept-Encoding: gzip
Connection: close
Content-Length: 1010
Content-Type: application/json;charset=UTF-8
User-Agent: xjy-service
X-Forwarded-For: 118.31.172.115
X-Forwarded-Host: demo.xiaoheiban.cn
X-Forwarded-Server: demo.xiaoheiban.cn
X-Request-Uri: /internal//email/send
X-Trace-Id: af8d3a1c11951df8868f2a6d9a2711fc

{"receivers":["dudao@xiaoheiban.cn","zhangyang@xiaoheiban.cn","shaqinyan@xiaoheiban.cn","chengyuan@xiaoheiban.cn"],"subject":"彭泽县第二中学开通班级考核权限","type":"html","body":"\u003chtml\u003e\n\t\t\t\t\t\u003cbody\u003e\n\t\t\t\t\t\t\u003cp\u003eHi ALL,\u003c/p\u003e\n\t\t\t\t\t\t\u003cp\u003e系统已自动开通权限，详情如下，如需关闭，请手动关闭。\u003c/p\u003e\n\t\t\t\t\t\t\u003cp\u003e合同（ID=6124）：\u003ca href=\"https://yt.xiaoheiban.cn/web/#/contract/\n\t\t\t\t\t\t\t\t6124\"\u003e 班级考核取消协议 \u003c/a\u003e \u003cbr/\u003e\n\t\t\t\t\t\t\t学校（ID=320252）：彭泽县第二中学 \u003cbr/\u003e\n\t\t\t\t\t\t\t项目：班级考核 \u003cbr/\u003e\n\t\t\t\t\t\t\t起止时间：2020-05-01 ~ 2022-05-01 \u003cbr/\u003e\n\t\t\t\t\t\t\t触发原因：程寽源刚刚点击了校方盖章寄出，快递：顺丰快递，单号：SF1192244887847。\u003cbr/\u003e\u003c/p\u003e\n\t\t\t\t\t\u003c/body\u003e\n\t\t\t\t\u003c/html\u003e"}
<= {"code":10001,"desc":"服务器竟然开小差，一会儿再试试吧"}



```
