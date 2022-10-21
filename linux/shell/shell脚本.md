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

## 查询当前使用了哪个shell

```
echo $SHELL 或者
env | grep SHELL 或者
echo $0
```

## shell变量的值是否需要加引号？

如果变量的值中不包含空格，则不需要加引号，否则必须加引号；其中单引号中是什么就打印什么，双引号则会解析其中的变量；

```
var1=abcd
var2="de fg"
var3='${var1}'
var4="${var1}"

echo $var1
echo $var2
echo $var3
echo $var4
```

打印结果为：

```
abcd
de fg
${var1}
abcd
```

## 如何引用变量？

- 方式一`$var`

- 方式二`${var}`
