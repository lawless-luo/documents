服务安装：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/index.html
java api：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.4/index.html
es权威指南：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

Indices-Alias：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/indices-aliases.html

## Alias管理
- 索引别名API允许使用名称别名索引，所有API都++会自动将别名转换为实际索引名++
- 别名还可以映射到多个索引，当指定它时，别名将自动扩展到别名索引
- 别名还可以与搜索时自动应用的筛选器和路由值关联
- 别名不能具有与索引相同的名称

下面是给索引取别名的例子：
```
POST /_aliases
{
    "actions" : [
        { "add" : { "index" : "test1", "alias" : "alias1" } }
    ]
}
```

下面是移除索引别名：
```
POST /_aliases
{
    "actions" : [
        { "remove" : { "index" : "test1", "alias" : "alias1" } }
    ]
}
```

在同一个API中，重命名别名是一个简单的删除然后添加操作。此操作是原子性的，无需担心短时间内别名不指向索引(这里相当于别名指向新的索引):
```
POST /_aliases
{
    "actions" : [
        { "remove" : { "index" : "test1", "alias" : "alias1" } },
        { "add" : { "index" : "test2", "alias" : "alias1" } }
    ]
}
```

将别名与多个索引关联只是几个添加操作:
```
POST /_aliases
{
    "actions" : [
        { "add" : { "index" : "test1", "alias" : "alias1" } },
        { "add" : { "index" : "test2", "alias" : "alias1" } }
    ]
}
```

利用索引数组语法通过一次操作给多个索引增加别名：
```
POST /_aliases
{
    "actions" : [
        { "add" : { "indices" : ["test1", "test2"], "alias" : "alias1" } }
    ]
}
```

在上面的例子中，一个glob模式也可以用来将一个别名关联到多个共享相同名称的索引:
```
POST /_aliases
{
    "actions" : [
        { "add" : { "index" : "test*", "alias" : "all_test_indices" } }
    ]
}
```

可以在一次操作中使用别名交换索引(不影响查询的情况下交换索引):
```
PUT test     
PUT test_2   
POST /_aliases
{
    "actions" : [
        { "add":  { "index": "test_2", "alias": "test" } },
        { "remove_index": { "index": "test" } }  
    ]
}
```

## Filters Aliases--过滤器别名
带有过滤器的别名提供了创建同一个索引的不同“视图”的简单方法。过滤器可以使用查询DSL定义，并应用于所有搜索、计数、按查询删除以及类似于此别名的操作。






























