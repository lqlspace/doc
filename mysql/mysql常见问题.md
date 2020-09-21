# 1. auto_increment
创建表时，如果某字段加了auto_increment，那么该字段必有index(不一定是primary key，但一般都是的)

# 2. not null default 
某字段如果定义了not null，一定要设定一个default value，不然，写入时db晓得赋何值；


