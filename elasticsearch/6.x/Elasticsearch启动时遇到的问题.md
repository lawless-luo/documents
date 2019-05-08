## 启动常见错误
- max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
```
vi  /etc/security/limits.conf

*               soft    nofile          65536
*               hard    nofile          65536
```

- max number of threads [3818] for user [es] is too low, increase to at least [4096]
```
vi  /etc/security/limits.conf

*               soft    nproc           4096
*               hard    nproc           4096

vi /etc/security/limits.d/90-nproc.conf

*          soft    nproc     4096
```

- max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
vi /etc/sysctl.conf

vm.max_map_count=262144
```

- system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
```
vi elasticsearch.yml

bootstrap.memory_lock: false 
bootstrap.system_call_filter: false
```

- can not run elasticsearch as root
1. 在执行elasticSearch时加上参数-Des.insecure.allow.root=true
```
./elasticsearch -Des.insecure.allow.root=true  
```

2. 用vim打开elasicsearch执行文件，在变量ES_JAVA_OPTS使用前添加以下命令
```
ES_JAVA_OPTS="-Des.insecure.allow.root=true"  
```

3. 添加用户
```
adduser es   //添加用户

passwd es  //给用户添加密码

chown -R 文件夹名 用户名 //将文件夹所有者改为指定用户

chmod 777 文件夹或文件  //将文件夹或文件的权限给用户

su es //切换用户

./bin/elasticsearch //启动elasticsearch 
```









