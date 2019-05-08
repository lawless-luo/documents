# Elasticsearch
## 官网地址
服务安装：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/index.html  
java api：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.4/index.html  
es权威指南：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

Set up Elasticsearch：https://www.elastic.co/guide/en/elasticsearch/reference/6.4/setup.html

## 安装Elasticsearch(linux安装)
- 下载
- 安装
- 启动
- 配置

## java(JVM)版本
至少java 8+。建议安装jdk 1.8.0_131+。


## 开始安装Elasticsearch
- 下载安装包并解压
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.3.tar.gz
tar -xzf elasticsearch-6.4.3.tar.gz
cd elasticsearch-6.4.3/
```

- 从命令行运行 Elasticsearch
```
./bin/elasticsearch
```
或者nohup后台运行Elasticsearch
```
nohup ./bin/elasticsearch &
pgrep -f elasticsearch
kill -9 pid
```
或者守护进程启动和停止
```
./bin/elasticsearch -d -p es.pid
kill -9 `es.pid`
```

- 检查Elasticsearch是否正在运行

**1.curl命令：**
```
curl -X GET "localhost:9200/"
```

**2.浏览器http请求:**
```
http://localhost:9200/
```
**3.kibana：**
```
GET /
```
响应结果：
```
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "6.4.3",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "f27399d",
    "build_date" : "2016-03-30T09:51:41.449Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "1.2.3",
    "minimum_index_compatibility_version" : "1.2.3"
  },
  "tagline" : "You Know, for Search"
}
```


## 解压后目录布局
- bin：elasticsearch相关脚本
- config：elasticsearch配置目录，配置文件包括 elasticsearch.yml
- data：数据存放目录
- logs：日志目录


## 配置elasticsearch
配置文件应该包含设置：
- node.name ：节点名
- cluster.name：集群名
- network.host：ip
- http.port：端口

#### 配置文件位置
Elasticsearch有三个配置文件(config/)：

- elasticsearch.yml：用来配置Elasticsearch
- jvm.options：配置Elasticsearch的jvm设置
- log4j2.properties：配置Elasticsearch日志


## 重要的 Elasticsearch配置
以下配置在使用前必须考虑到：

- Path settings:
```
path.data: /path/to/data
path.logs: /path/to/logs
```

- Cluster name:
```
cluster.name: my-elasticsearch
```

- Node name:
```
node.name: node-1
```

- Network host：
```
network.host: 192.168.1.10
```

- Discovery settings：
```
discovery.zen.ping.unicast.hosts:
   - 192.168.1.10:9300
   - 192.168.1.11 
   - seeds.mydomain.com 
```

- Heap size
```
-Xms2g
-Xmx2g 
```

- Heap dump path
- GC logging
- Temp directory
































