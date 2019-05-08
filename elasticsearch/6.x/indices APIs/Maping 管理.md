# Elasticsearch
## 官网地址
服务安装：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/index.html  
java api：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.4/index.html  
es权威指南：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

Indices APIs：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/indices.html

## Indices APIs
indices api用于管理单个索引、索引设置、别名、映射和索引模板。


## Mapping 管理
- Put Mapping
- Get Mapping
- Get Field Mapping
- Types Exists


## Put Mapping
PUT mapping API允许您向现有索引添加字段或仅更改已存在字段的搜索设置  
下面的例子是用来创建Mapping的：
```
PUT twitter 
{}

PUT twitter/_mapping/_doc 
{
  "properties": {
    "email": {
      "type": "keyword"
    }
  }
}
```

==note：==

- 第一个：创建了一个没有mapping的twitter索引。
- 第二个：使用PUT mapping API向_doc映射类型添加一个名为email的新字段

## Multi-Index
PUT mapping API在一个请求中可以用在多个索引里。  
下面的例子我们可以同时更新twitter-1 和 twitter-2 的mappings：
```
# Create the two indices
PUT twitter-1
PUT twitter-2

# Update both mappings
PUT /twitter-1,twitter-2/_mapping/_doc 
{
  "properties": {
    "user_name": {
      "type": "text"
    }
  }
}
```

==note：== 
- 指定的索引(twitter-1、twitter-2)遵循多个索引名和通配符形式
- 当使用PUT映射API更新_default_映射时，新映射不会与现有映射合并。相反，新的_default_映射替换了现有的映射。

## Get Mapping--获取Mapping
获取索引或类型的mapping：
```
GET /twitter/_mapping/_doc
```

## Multiple indices and types--多个索引和类型
==note：==

- 一次调用可以获取多个索引或类型
- 使用遵循规则：host:port/{index,index1,...}/_mapping/{type,type1,...}
- 得到所有索引mapping：_all替换上面的{index}
具体示例(一下两种方式是等价的)：
```
GET /_mapping/_doc

GET /_all/_mapping/_doc
```

## Get Field Mapping--获取属性Mapping
首先创建一个索引并同时创建一个Mapping：
```
PUT publications
{
    "mappings": {
        "_doc": {
            "properties": {
                "id": { "type": "text" },
                "title":  { "type": "text"},
                "abstract": { "type": "text"},
                "author": {
                    "properties": {
                        "id": { "type": "text" },
                        "name": { "type": "text" }
                    }
                }
            }
        }
    }
}
```

返回title字段的mapping：
```
GET publications/_mapping/_doc/field/title
```

响应结果：
```
{
  "publications": {
    "mappings": {
      "_doc": {
        "title": {
          "full_name": "title",
          "mapping": {
            "title": {
              "type": "text"
            }
          }
        }
      }
    }
  }
}
```

## Mutiple Indices Types和Fields
- 使用规则：host:port/{index,index1...}/{type,type1...}/_mapping/field/{field,field1...}

```
GET /twitter,kimchy/_mapping/field/message

GET /_all/_mapping/_doc/field/message,user.id

GET /_all/_mapping/_doc/field/*.id
```

下面示例指定字段的mapping获取：
```
GET publications/_mapping/_doc/field/author.id,abstract,name
```

响应结果：
```
{
  "publications": {
    "mappings": {
      "_doc": {
        "abstract": {
          "full_name": "abstract",
          "mapping": {
            "abstract": {
              "type": "text"
            }
          }
        },
        "author.id": {
          "full_name": "author.id",
          "mapping": {
            "id": {
              "type": "text"
            }
          }
        }
      }
    }
  }
}
```

下面示例展示了字段mapping通配符匹配：
```
GET publications/_mapping/_doc/field/a*
```

响应结果：
```
{
  "publications": {
    "mappings": {
      "_doc": {
        "author.name": {
          "full_name": "author.name",
          "mapping": {
            "name": {
              "type": "text"
            }
          }
        },
        "abstract": {
          "full_name": "abstract",
          "mapping": {
            "abstract": {
              "type": "text"
            }
          }
        },
        "author.id": {
          "full_name": "author.id",
          "mapping": {
            "id": {
              "type": "text"
            }
          }
        }
      }
    }
  }
}
```

## Types Exists--Types是否存在
```
HEAD twitter/_mapping/tweet
```























