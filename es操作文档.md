## 根据campus_id查询学校
```cassandraql
GET school/_search
{
"query":{
        "bool":{
            "must":[
                {
                    "terms":{
                        "campus.campus_id":[
                            10008
                        ]
                    }
                }
            ]
        }
    }
}
```
