## mysql初始化
a. canal的原理是基于mysql binlog技术，所以这里一定需要开启mysql的binlog写入功能，建议配置binlog模式为row.  
具体步骤如下：
```
vi /etc/my.cnf

[mysqld]
log-bin=mysql-bin #添加这一行就ok
binlog-format=ROW #选择row模式
server_id=1 #配置mysql replaction需要定义，不能和canal的slaveId重复
```

b.canal的原理是模拟自己为mysql slave，所以这里一定需要做为mysql slave的相关权限.  
具体步骤如下：
```
//创建一个canal用户
CREATE USER canal IDENTIFIED BY 'canal';  
//赋予权限
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
//刷新权限
FLUSH PRIVILEGES;
```
==note:== 针对已有的账户可直接通过grant












































