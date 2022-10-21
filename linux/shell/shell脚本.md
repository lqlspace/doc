## shell变量设置默认值

- a没有定义或者为空字符串时，表达式返回默认值，否则返回a的值

```
#!/bin/sh
a="hello"

ret=${a:-"/usr/local"}
echo "ret: " $ret
```

- a没有定义时，表达式返回默认值，否则返回a的值

```
#!/bin/sh
a=""

ret=${a-"/usr/local"}
echo "ret: " $ret
```


