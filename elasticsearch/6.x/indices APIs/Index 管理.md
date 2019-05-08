# Elasticsearch
## 官网地址
服务安装：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/index.html  
java api：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.4/index.html  
es权威指南：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

Indices APIs：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/indices.html

## Indices APIs
indices api用于管理单个索引、索引设置、别名、映射和索引模板。


## index 管理
- Create Index
- Delete Index
- Get Index
- Indices Exists
- Open / Close Index API
- Shrink Index
- Split Index
- Rollover Index


## Create Index--创建索引
Create Index API用于在Elasticsearch中手动创建索引。Elasticsearch中的所有文档都存储在一个或另一个索引中。

基本命令：
```
PUT twitter
```

==note：== 这里创建了一个名字为twitter的索引，它的其他设置为默认设置。

#### 索引名称限制
- 只接受小写字母
- 不能包括的字符有：\, /, *, ?, ", <, >, |, ` `(空格),，,#等
- 7.0之前的索引包含冒号(:)，现在已经过时并且在7.0已经不支持了。
- 不能以-, _, +这些开头
- 不能是.或者..
- 不能超过255字节(注意它是字节，所以多字节字符将更快地计算到255的极限)


#### 索引设置
创建的每个索引都可以有与之相关的特定设置，在主体中定义:
```
PUT twitter
{
    "settings" : {
        "index" : {
            "number_of_shards" : 3, 
            "number_of_replicas" : 2 
        }
    }
}
```
或者更简化
```
PUT twitter
{
    "settings" : {
        "number_of_shards" : 3,
        "number_of_replicas" : 2
    }
}
```

==note：== 

- number_of_shards默认为5。
- number_of_replicas默认为1(每个主shard一个replica)


#### Mappings--映射
create index API允许提供一个类型mapping：
```
PUT test
{
    "settings" : {
        "number_of_shards" : 1
    },
    "mappings" : {
        "_doc" : {
            "properties" : {
                "field1" : { "type" : "text" }
            }
        }
    }
}
```


#### Aliases--别名
create index API还允许提供一组别名:
```
PUT test
{
    "aliases" : {
        "alias_1" : {},
        "alias_2" : {
            "filter" : {
                "term" : {"user" : "kimchy" }
            },
            "routing" : "kimchy"
        }
    }
}
```

## Delete Index--删除索引
==note：==   
- 可以指定一个index或者一个通配符表达式。
- aliases不能用来删除index。
- 通配符表达式被解析后用来匹配具体的index。
- delete index API还可以应用于多个索引，方法是使用逗号分隔的列表，或者使用_all或*作为索引应用于所有索引(注意!)。
- 为了禁用允许通过通配符或_all删除索引，可以设置action.destructive_requires_name = true

下面的例子用于删除已存在的索引
```
DELETE /twitter
```


## Get Index--获取索引
下面的例子用来检索一个或多个Index
```
GET /twitter
```

==note：==

- 上面的例子用来获取名字叫twitter的信息。
- 可以指定一个index名，别名或者通配符表达式。
- get index API可以用来作用一个或多个Index或者用_all、*作用于所有index。


## indices exists-索引是否存在
下面的例子用于检查索引是否存在：
```
HEAD twitter
```

==note：==

- 404：索引不存在
- 200：索引存在
- 此请求不区分index和alias。如果alias名存在也会返回200。

## 打开或关闭索引













































