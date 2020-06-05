## 查询表中某字段去重列表
> 方式1：采用distinct
```cassandraql
 SELECT  distinct user_id
FROM hera.user
WHERE create_time >= toStartOfDay(toDate('2020-05-01'))
  AND create_time < addDays(toStartOfDay(toDate('2020-05-01')), 1)
  AND (event = '/POST:::/trace/device/enter-background' OR
    event = '/POST:::/api/trace/device/enter-background')
```
> 方式2：采用group by
```cassandraql
SELECT  user_id
FROM hera.user_operation_all
WHERE create_time >= toStartOfDay(toDate('2020-05-01'))
  AND create_time < addDays(toStartOfDay(toDate('2020-05-01')), 1)
  AND (event = '/POST:::/trace/device/enter-background' OR
       event = '/POST:::/api/trace/device/enter-background')
GROUP BY user_id
```

## 查询insert_id最大的哪条记录
```cassandraql
SELECT id AS                              campus_id,
     argMax(xjy_create_time, insert_id) campus_create_time,
     argMax(invited_number, insert_id)  invited_number
FROM campus_now
WHERE xjy_create_time != '0000-00-00 00:00:00'
and campus_id != 0
GROUP BY campus_id
```
