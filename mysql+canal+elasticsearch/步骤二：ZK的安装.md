## Zookeeper
## 官网地址
get started：https://zookeeper.apache.org/doc/current/zookeeperStarted.html  
zk release：http://zookeeper.apache.org/releases.html  
old release：https://archive.apache.org/dist/zookeeper/

## 下载安装
```
wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz

tar -xzvf zookeeper-3.4.14.tar.gz
cd zookeeper-3.4.14
```

## 配置
1.创建一个文件名为zoo.cfg的配置文件
```
touch conf/zoo.cfg

tickTime=2000
dataDir=/var/lib/zookeeper 
clientPort=2181
```
==note== 

- tickTime：ZooKeeper使用的以毫秒为单位的基本时间单位。它用于心跳，最小会话超时将是tickTime的两倍
- dataDir：存储内存中数据库快照，以及更新到数据库的事务日志(除非另有指定)的位置。
- clientPort：监听客户机连接的端口

## 启动Zookeeper
```
bin/zkServer.sh start
```


## 连接到Zookeeper
```
bin/zkCli.sh -server 127.0.0.1:2181
```
连接上会有以下响应结果：
```
Connecting to localhost:2181
log4j:WARN No appenders could be found for logger (org.apache.zookeeper.ZooKeeper).
log4j:WARN Please initialize the log4j system properly.
Welcome to ZooKeeper!
JLine support is enabled
[zkshell: 0]
```


























