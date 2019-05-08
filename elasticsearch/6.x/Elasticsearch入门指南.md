# Elasticsearch
## 官网地址
服务安装：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/index.html  
java api：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.4/index.html  
es权威指南：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

Getting Started: https://www.elastic.co/guide/en/elasticsearch/reference/6.4/getting-started.html

## 简要介绍
Elasticsearch是一个高度可伸缩的开源全文搜索和分析引擎。它允许您快速、实时地存储、搜索和分析大量数据。它通常用作底层引擎/技术，为具有复杂搜索特性和需求的应用程序提供动力。

## 基本概念
- 近实时(near realtime)
- 集群(cluster)
- 节点(node)
- 索引(index)
- 类型(type)
- 文档(document)
- 分片&复制集(shards&replicas)

## 安装
Elasticsearch要求jdk版本: java 8 +。  
安装前可以检查下java版本：
```
java -version
echo $JAVA_HOME
```

**使用tar安装的示例**

- 下载Elasticsearch 6.4.3 tar
```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.3.tar.gz
```

- 解压包
```
tar -xvf elasticsearch-6.4.3.tar.gz
```

- 启动
```
cd elasticsearch-6.4.3/bin
./elasticsearch
```

## 集群健康
1.检查集群健康
```
curl -X GET "localhost:9200/_cat/health?v"
```
或者在kibana上的Dev Tools 执行下面命令：
```
GET /_cat/health?v
```

响应结果：
```
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1475247709 17:01:49  elasticsearch green           1         1      0   0    0    0        0             0                  -                100.0%
```

结果说明：

- green 代表一切正常（集群功能健全）。
- yellow 所有数据都可用，但一些replicas未作分配(集群功能健全)
- red 因为某些原因，一些数据不可用(集群具有部分功能)


2.列出集群节点
```
curl -X GET "localhost:9200/_cat/nodes?v"
```
或者在kibana上的Dev Tools 执行下面命令：
```
GET /_cat/nodes?v
```

响应结果：
```
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           10           5   5    4.46                        mdi      *      PB2SGZY
```

我们可以看到一个名为“PB2SGZY”的节点


## 列出所有索引
现在让我们来看看我们的索引：
```
curl -X GET "localhost:9200/_cat/indices?v"
```
或者在kibana上的Dev Tools 执行下面命令：
```
GET /_cat/indices?v
```

响应结果：
```
health status index       uuid                   pri rep docs.count docs.deleted store.size pri.store.size
```

这里说明目前还没有创建一个索引。


## 创建索引
让我们来创建一个名为"customer"的索引，然后列出所有的索引：
```
curl -X PUT "localhost:9200/customer?pretty"
curl -X GET "localhost:9200/_cat/indices?v"
```
或者在kibana上的Dev Tools 执行下面命令：
```
PUT /customer?pretty
GET /_cat/indices?v
```

第一条命令用PUT创建了"customer"索引，我们可以在调用末尾加上pretty 漂亮的打印出JSON结果：
```
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "customer"
}
```

第二条命令告诉我们已经有一个customer的索引了，并且它有5个primary shards和一个replica(默认)，还包含了0个documents
```
health status index    uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   customer 95SQ4TSUT7mWBT7VNHH67A   5   1          0            0       260b           260b
```

## 索引和查询文档
让我们put某些内容到customer索引里，这里put一个简单的文档，这个文档的ID为1：
```
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}
```

响应结果：
```
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
从上面可以看出，一个新的customer document在customer index中成功的建立了，文档的内部id也是1，这是我们在索引时指定的。

让我们来检索我们索引的文档：
```
curl -X GET "localhost:9200/customer/_doc/1?pretty"
```
或者在kibana上的Dev Tools 执行下面命令：
```
GET /customer/_doc/1?pretty
```

响应结果：
```
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "_source" : { "name": "John Doe" }
}
```

## 删除索引
现在让我们删除刚才创建好的索引，然后列出所有的索引：
```
curl -X DELETE "localhost:9200/customer?pretty"
curl -X GET "localhost:9200/_cat/indices?v"
```
或者在kibana上的Dev Tools 执行下面命令：
```
DELETE /customer?pretty
GET /_cat/indices?v
```

响应结果：
```
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
```
这意味着索引已经成功的被删除了，并且我们回到了集群最开始什么都没有的时候。
在我们进入下一步之前，让我们再仔细看看我们目前为止学过的API 命令：
```
curl -X PUT "localhost:9200/customer"
curl -X PUT "localhost:9200/customer/_doc/1" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
'
curl -X GET "localhost:9200/customer/_doc/1"
curl -X DELETE "localhost:9200/customer"
```
或者在kibana上的Dev Tools 执行下面命令：
```
PUT /customer
PUT /customer/_doc/1
{
  "name": "John Doe"
}
GET /customer/_doc/1
DELETE /customer
```

## 创建或替换档
之前，我们知道了如何创建一个简单的document。让我们再recall一次:
```
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}
```

现在我们可以再次用不同的(或相同的)document执行一次命令，elasticsearch会替换掉已经存在的ID为1的document：
```
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "Jane Doe"
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
PUT /customer/_doc/1?pretty
{
  "name": "Jane Doe"
}
```

当创建索引的时候，ID部分是可选的。如果不指定，elasticsearh会随机生成一个ID，用这个ID去索引文档。

## 更新文档
下面的例子展示了通过改变name属性为"Jane Doe"去如何更新我们之前的文档：
```
curl -X POST "localhost:9200/customer/_doc/1/_update?pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe" }
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}
```

下面的例子展示了如何更新ID为1的document，通过改变name和添加age：
```
curl -X POST "localhost:9200/customer/_doc/1/_update?pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
```

也可以通过script去执行更新。下面的示例通过script让年龄加5：
```
curl -X POST "localhost:9200/customer/_doc/1/_update?pretty" -H 'Content-Type: application/json' -d'
{
  "script" : "ctx._source.age += 5"
}
'
```
或者在kibana上的Dev Tools 执行下面命令：
```
POST /customer/_doc/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```

## 删除文档
下面的例子就是展示如何删除ID为2的customer：
```
curl -X DELETE "localhost:9200/customer/_doc/2?pretty"
```
或者在kibana上的Dev Tools 执行下面命令：
```
DELETE /customer/_doc/2?pretty
```

## 批处理
==注：后面的文档示例不会再用curl，只会用kibana上执行的命令。==  
下面的调用会用bulk操作创建两个文档，分别是ID为1-John Doe和ID为2-Jane Doe ：
```
POST /customer/_doc/_bulk?pretty
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
```

下面的例子会用bulk操作更新ID为1的第一个文档并且删除ID为2的第二个文档：
```
POST /customer/_doc/_bulk?pretty
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```
bulk API不会因为一个操作失败而全部失败。如果一次操作因为某些原因失败，剩下的操作会继续处理。返回的结果会给
每个操作提供一个状态，这样你就可以知道操作是否成功了。

kibana测试请求：POST /bank/_doc/_bulk?pretty&refresh  
测试数据地址：https://github.com/lawless-luo/elasticsearch/blob/master/docs/src/test/resources/accounts.json 

## 查询API

查询方式有两种：一种是在request URI上发送查询参数，另一种是通过request body发送查询参数。

下面例子返回bank index中的所有documents:
```
GET /bank/_search?q=*&sort=account_number:asc&pretty
```
调用分析：GET /bank/_search?q=*&sort=account_number:asc&pretty
- q=*：表示match all documents
- sort=account_number:asc：表示通过account_number字段升序
- pretty：表示结果按漂亮的JSON返回
- &：参数连接符号

响应结果：
```
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```
结果分析：

- took：elasticsearch执行搜索的时间
- timed_out：告诉我们搜索是否超时
- _shards：告诉我们多少shards被搜索，以及成功和失败的shards
- hits：搜索的结果
- hits.total：符合搜索条件的文档总数
- hits.hits：搜索结果实际数组(默认前10个文档)
- hits.sort：结果排序键(如果按分数排序，则丢失)
- hits._score和max_score：暂时忽略这些字段


下面是使用request body方法搜索：
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```
这里没有用q=*,而是通过json风格的query request body。


## 介绍查询语言
回到我们上个例子，我们执行这个查询：
```
GET /bank/_search
{
  "query": { "match_all": {} }
}
```
==note==：这里是代表查询所有index中的documents。

除了query参数，我们可以传递其他的参数去影响搜索结果。
下面的例子中我们增加了一个size参数：
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "size": 1
}
```
==note==：这里的size不指定的话，默认为10。

下面的例子做了一个match_all，并且返回10-19之间的documents：
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}
```

==note：==

- from：从0开始的参数，表示从哪个文档开始，默认为0。
- size：从from参数开始返回多少文档  
以上参数可用于分页搜索


下面的例子执行match_all操作，并按帐户余额降序对结果进行排序，并返回前10个(默认大小)文档：
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } }
}
```


## 执行搜索

- 下面的例子展示了如何从搜索中返回2个字段，account_number和balance(_source内部)：
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}
```

==note：==     
上面的字段简单的减少了_source字段，返回的字段中只包括了account_number和balance。  
类似于SQL语句：select from 字段列表。

- 下面的例子返回account number为20的数据：
```
GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}
```

- 下面的例子返回address字段中包含术语"mill"的所有accounts：
```
GET /bank/_search
{
  "query": { "match": { "address": "mill" } }
}
```

- 下面的例子返回address中包含术语"mill"或"lane"的所有accounts：
```
GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
```

- 下面的例子是match(match_phrase)的变体，它返回address中包含短语"mill lane"的所有accounts。
```
GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
```


- **bool 查询**  
**1. bool must**--下面的例子组合了两个match查询并且返回了address中包含"mill"和"lane"的所有accounts：
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
```
==note：== 上面的示例中,bool must指定的所有查询都必须满足，才能匹配出结果。

2.bool should--下面的例子组合了两个match查询并且返回address中包含"mill"或"lane"的所有accounts：
```
GET /bank/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
```
==note：== 上面的示例中,bool should指定的列表查询中满足任一一个，才能匹配出结果。

3.bool must_not--下面的例子组合了两个match查询并且返回address中不包含"mill"和"lane"的所有accounts：
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
```
==note：== 上面的示例中,bool must_not指定的列表查询中都不满足，才能匹配出结果。



## 执行过滤器
_score:

- score是一个衡量文档与搜索匹配程度的相对指标的数值
- score越高，文档相关性越高；score越低，文当相关性越低

**1.range query**--通常用于数字和日期过滤  
    下面的例子使用了bool query返回balances在[2000,30000]之间的所有accounts：
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```

## 执行聚合

聚合提供了对数据进行分组和提取统计信息的能力。考虑聚合最简单的方法是将其大致等同于SQL GROUP by和SQL聚合函数。  
**1.** 下面的示例按state字段对所有accounts进行分组，然后返回按count降序排列的前10个(默认)状态(也是默认):
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}
```

==note：== 上面的聚合类似于：
```
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC LIMIT 10;
```

响应结果：
```
{
  "took": 29,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped" : 0,
    "failed": 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "doc_count_error_upper_bound": 20,
      "sum_other_doc_count": 770,
      "buckets" : [ {
        "key" : "ID",
        "doc_count" : 27
      }, {
        "key" : "TX",
        "doc_count" : 27
      }, {
        "key" : "AL",
        "doc_count" : 25
      }, {
        "key" : "MD",
        "doc_count" : 25
      }, {
        "key" : "TN",
        "doc_count" : 23
      }, {
        "key" : "MA",
        "doc_count" : 21
      }, {
        "key" : "NC",
        "doc_count" : 21
      }, {
        "key" : "ND",
        "doc_count" : 21
      }, {
        "key" : "ME",
        "doc_count" : 20
      }, {
        "key" : "MO",
        "doc_count" : 20
      } ]
    }
  }
}
```

==note：== 注意我们设置size=0 为了不展示搜索hits，因为我们只想看聚合结果。

**2.** 基于上面的例子，下面的示例按state字段计算account平均余额(按count降序取前面10个状态)：
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```

==note：== 上面的例子是在group_by_state 聚合内嵌套的average_balance聚合。这是所有聚合的共同模式。我们可以在聚合中嵌套聚合。

**3.** 基于上面的聚合，我们按照平均余额降序排序。
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```


**4.** 下面的例子演示了我们如何通过年龄组(ages 20-29,30-39,40-49)分组,  
然后通过性别分组,最后获取每个年龄组对应性别的平均账户余额：
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}
```

































