## Kibana
## 官网地址
Kibana用户指南：https://www.elastic.co/guide/en/kibana/6.4/index.html

## 介绍
Kibana是一个开放源码的分析和可视化平台，设计用于Elasticsearch。使用Kibana搜索、查看和与存储在Elasticsearch索引中的数据交互。您可以轻松地执行高级数据分析，并在各种图表、表格和映射中可视化数据。
Kibana使大量数据易于理解。它的简单、基于浏览器的界面使您能够快速创建和共享动态仪表板，实时显示对Elasticsearch查询的更改。
安装kibana很容易。您可以安装Kibana并在几分钟内开始研究您的Elasticsearch索引——不需要代码，也不需要额外的基础设施。

## 安装Kibana
- 下载
- 安装
- 开始
- 配置
- 升级


#### Elasticsearch 版本
应该将Kibana配置为针对相同版本的Elasticsearch节点运行。这是官方支持的配置。

#### 开始安装 Kibana
注意：从6.0.0版本开始，kibana只支持64位的操作系统

#### 开始安装
- 下载和安装Kibana linux 64bit 包
```
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.4.3-linux-x86_64.tar.gz
tar -xzf kibana-6.4.3-linux-x86_64.tar.gz
cd kibana-6.4.3-linux-x86_64/ 
```

- 启动和停止kibana
```
./bin/kibana

pgrep -f kibana
kill -9 pid
```
或者
```
nohup ./bin/kibana &

lsof -i :35601 | awk '{print $2}'
kill  -9  pid
```


- 访问kibana
```
localhost:5601 或者 http://YOURDOMAIN.com:5601.
```

- 检查kibana状态
```
localhost:5601/status
```


## 配置Kibana
配置文件：kibana.yml。通常与elasticsearch结合使用时，简单配置如下：

- server.port: 35601
- server.host: "lawless.vm.in"
- elasticsearch.url: "http://lawless.vm.in:39200"