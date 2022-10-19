## 形式

```
TARGET: DEPENDENCES
    CMD
```

> TARGET: 生成的目标，可以是一个文件，也可以是一个虚拟符号（非真实文件）；
> 
> DEPENDENCES: 生成目标的所有依赖，它是一个集合，可以有一个文件，也可以多个文件，也可以是虚拟符号；
> 
> CMD：执行命令，CMD前是一个TAB键，不是空格；
> 
> 基本规则：当一个TARGET （欲生成的目标）比它的任何一个DEPENDENCES（依赖的文件）旧（时间差）时，这个TARGET就要重新生成；



## 如何调试Makefile？

```
$(info "This is a info msg ...")
$(warning "This is a warning msg ...")
$(error "This is a error msg ... It will make this Makefile to exist beacuse of an 'error' accur.")
```

info，warning，error；这三个函数是Makefile中内置的，在任意Makefile中都可以使用。但是需要注意的，这三个函数可以在Makefile的任意位置调用，但是必须符合语法规则，比如在CMD部分调用时，必须以TAB开头。

## Makfile与shell的差异

- 在 Makefile 中可以调用 shell 脚本/命令；

- shell 不允许 “=” 号两边有空格；Makefile 允许变量赋值时，“=” 号两边留空格；

- shell 中通配符 * 表示所有的字符，Makefile 中通配符 % 表示所有的字符；

- shell中所有引用以$打头的变量其后要加{},而在Makefile中则是加()；

- Makefile中所有以`$`打头的单词都会被解释成Makefile中的变量。如果需要调用shell中的变量，都需要加两个`$$`。

## 打印输出

在Makefile中只能在target中调用shell脚本，其他地方是不能输出的，如下没有输出。

```
VAR="Hello"
echo "$VAR"
all:
```

如下则可以输出：

```
VAR="Hello"
all:
    echo "$VAR"
```



## Makefile定义变量的时候调用shell命令

- 形式一
  
  ```
  CUR_PATH = `pwd`
  ```

        使用反括号调用shell命令，并将返回值赋给变量CUR_PATH。

- 形式二
  
  ```
  LS_FILES = "`ls`"
  ```

        此处用双引号包裹，是因为ls的返回值通常是一个字符串，中间每个文件名用空格隔开的。如果我们想把这一系列的字符串连起来而不是分散的，我们就必须使用 " "把它们包起来，这样Makefile变量就会把它当作一个字符串整体来处理。

- 形式三
  
  ```
  LS_TEST_FILES = "$(shell ls test*)"
  ```
  
  采用格式是 $(shell xxx)格式，其中xxx就是一个可以在shell环境下执行的shell命令。

## Makefile在执行命令的时候调用shell脚本

```
shell_cmd:
    @echo "echo cmd"
    @date
```

shell命令前添加@符号；
