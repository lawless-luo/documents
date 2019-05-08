## canal
## 官网地址
wiki quickstart：https://github.com/alibaba/canal/wiki/QuickStart  
release版本：https://github.com/alibaba/canal/releases

## 下载canal
```
wget https://github.com/alibaba/canal/releases/download/canal-1.1.3/canal.deployer-1.1.3.tar.gz
```
或者自己编译
```
git clone git@github.com:alibaba/canal.git
cd canal; 
mvn clean install -Dmaven.test.skip -Denv=release
```
编译完成后，会在根目录下产生target/canal.deployer-1.1.3.tar.gz


## 解压缩
```
mkdir /tmp/canal
tar zxvf canal.deployer-1.1.3.tar.gz  -C /tmp/canal
cd canal.deployer-1.1.3
```
解压后会看到以下目录：
```
drwxr-xr-x 2 jianghang jianghang  136 2013-02-05 21:51 bin
drwxr-xr-x 4 jianghang jianghang  160 2013-02-05 21:51 conf
drwxr-xr-x 2 jianghang jianghang 1.3K 2013-02-05 21:51 lib
drwxr-xr-x 2 jianghang jianghang   48 2013-02-05 21:29 logs
```

## 配置canal
1.conf/canal.properties
```
vi conf/canal.properties

canal.zkServers=127.0.0.1:2181
```

2.conf/example/instance.properties
```
vi conf/example/instance.properties


## mysql serverId
canal.instance.mysql.slaveId=1234
# position info 需要改成自己的数据库信息 master库
canal.instance.master.address=localhost:3306

# username/password 需要改成自己的数据库信息
canal.instance.dbUsername=canal
canal.instance.dbPassword=canal

#table regex 需要改成对应数据库的table正则信息，多个以逗号分割
canal.instance.filter.regex = lawless.device_info
```

