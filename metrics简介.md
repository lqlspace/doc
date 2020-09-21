## metric含义
metric中文意思是指标，是监控系统中的基本单元，一条metric相当于db表里的一条记录，因为现在大部分metric最终也都是存在某一种db中，所以也只是换了个名字。
一张表有很多字段(column)，对应metric中有很多标签(label/tag)。

### step1:
我们想存下cpu各个时刻load的信息，例如:
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.2)  
- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.1)  
如果把这个metric存入db中，只要id，name，value，timestamp就够了  

| id | name     | value | timestamp  |   |
|----|----------|-------|------------|---|
| 1  | cpu.load | 0.2   | 1512982800 |   |
| 2  | cpu.load | 0.1   | 1512982801 |   |
|    |          |       |            |   |

### step2:
某一时刻，突然cpu.load飚到了50，我们想知道是哪一台服务器的load这么高，上表记录的数据就不够了, 那我们就要加一个服务器的标识：
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.1, instance=172.14.200.10)  
- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.1, instance=172.14.200.11)  
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.05, instance=172.14.200.10)  
-- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.05, instance=172.14.200.11)  

| id | name     | value | timestamp  | instance      |
|----|----------|-------|------------|---------------|
| 1  | cpu.load | 0.1   | 1512982800 | 172.14.200.10 |
| 2  | cpu.load | 0.1   | 1512982800 | 172.14.200.11 |
| 3  | cpu.load | 0.05  | 1512982801 | 172.14.200.10 |
| 4  | cpu.load | 0.05  | 1512982801 | 172.14.200.11 |
我们新增了一个instance字段来记录服务器ip，用于定位是哪台服务器，俩台机器 * 俩个时刻 = 4个metric。

### step3:
既然有了服务器信息，那么顺便把服务器上跑的服务名也加上方便排查吧，那我们又要新增一个服务名字段service  
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.2, instance=172.14.200.10，service=print)  
- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.1, instance=172.14.200.11，service=search)  
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.2, instance=172.14.200.10，service=print)  
- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.1, instance=172.14.200.11，service=search)  

| id | name     | value | timestamp  | instance      | service |
|----|----------|-------|------------|---------------|---------|
| 1  | cpu.load | 0.1   | 1512982800 | 172.14.200.10 | print   |
| 2  | cpu.load | 0.1   | 1512982800 | 172.14.200.11 | search  |
| 3  | cpu.load | 0.05  | 1512982801 | 172.14.200.10 | print   |
| 4  | cpu.load | 0.05  | 1512982801 | 172.14.200.11 | search  |

### step4:
不难发现，当指标确定后，name和value不易变化，大多数改变是在对metric添加一些额外的信息（intance、service），所以干脆把额外的属性都存成json类型，那么就可以随意填写属性条数，同一个metric下每一条记录的tag也不需要相同，我们把这个json体存到tag(或label)字段。
- 2017-12-11 17:00:00时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982800, value=0.2,tag{instance=172.14.200.10，service=print})  
- 2017-12-11 17:00:01时刻cpu.load的值，用mertic表述就是metric(name=cpu.load, timestamp=1512982801, value=0.1,tag{instance=172.14.200.11，service=search})

| id | name     | value | timestamp  | tag                                       |
|----|----------|-------|------------|-------------------------------------------|
| 1  | cpu.load | 0.1   | 1512982800 | {instance：172.14.200.10，service：print} |
| 2  | cpu.load | 0.1   | 1512982800 | {instance：172.14.200.11，service:search} |
| 3  | cpu.load | 0.05  | 1512982801 | {instance：172.14.200.10，service：print} |
| 4  | cpu.load | 0.05  | 1512982801 | {instance：172.14.200.11，service:search} |
最终我们得到了通用的metric结构，name，tag{}，value，timestamp（记住这个结构。记住这个结构。记住这个结构。），通过tag支持灵活的扩展字段。

### step5: 
解决了存储问题，那么要如何查询？
对于存储在DB里的metric，使用的就是SQL语法，也有些监控有用自定义的DSL，但查询思想大同小异。
> 想了解2017-12-11 17:00:00 - 2017-12-11 17:10:00的172.14.200.11这台服务器的总load情况
```
select name,sum(value) from metric where name = 'cpu.load' where timestamp between 1512982800 and 1512983400 group by tag.instance;
```
> 想了解2017-12-11 17:00:00 - 2017-12-11 17:10:00的print这个服务的总最大load情况:
```cassandraql
select name,max(value) from metric where name = 'cpu.load' and tag.service = 'print' where timestamp between 1512982800 and 1512983400 group by tag.service;
```
当然，查询的时候不聚合（group by ）也可以，但是得到的结果太过分散。


