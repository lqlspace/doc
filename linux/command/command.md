## 快捷键

- ctrl + a : 光标移至行首；

- ctrl + e：光标移至行尾；

- option + <-：光标以单词为单位向左移动；

- option + ->：光标以单词为单位向右移动；

## find命令

### 查找某个目录下的文件

```
find .git/objects -type f
```



## 查看内核版本和发行版本

- uname -a

> 1. 查看kernel名称、版本；2. 查看节点名称；3. 查看体系架构

```
kernel-name nodename kernel-release kernel-version
machine processor hardware-platform operating-system
```

![](/Users/lql/Library/Application%20Support/marktext/images/2022-11-24-09-05-43-image.png)

- cat /proc/version
1. 查看内核版本；2.查看发行版本；3.查看编译器版本

```
Linux version 4.15.0-194-generic (buildd@lcy02-amd64-052) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #205-Ubuntu SMP Fri Sep 16 19:49:27 UTC 2022
```

![](/Users/lql/Library/Application%20Support/marktext/images/2022-11-24-09-06-20-image.png)

- cat /etc/issue

![](/Users/lql/Library/Application%20Support/marktext/images/2022-11-24-09-04-26-image.png)

- lsb_release -a

![](/Users/lql/Library/Application%20Support/marktext/images/2022-11-24-09-07-07-image.png)
