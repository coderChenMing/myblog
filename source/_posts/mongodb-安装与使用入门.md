---
title: mongodb 安装与使用入门
date: 2019-01-03 22:45:50
updated: 2019-01-03 23:51:01
tags: mongo
categories: mongo
---
## 下载安装文件
    wget ftp://192.168.205.11/Third_party/linux/mongodb-linux-x86_64-rhel62-3.4.2.gz
## 安装
   ### (1).解压安装文件
    tar -xzvf mongodb-linux-x86_64-rhel62-3.4.2.gz -C /usr/local/eversec/
    cd /usr/local/eversec/
    mv mongodb-linux-x86_64-rhel62-3.4.2 mongodb
    cd /usr/local/eversec/mongodb
    cp bin/* /usr/local/bin
   ### (2).创建目录
     创建数据存放目录，例如：mkdir -p /home/mongodb/data
     创建日志存放目录，例如：mkdir -p /home/mongodb/logs
     创建pidfile文件路径，例如：mkdir /var/run/mongodb
     创建conf存放目录，例如：mkdir /usr/local/eversec/mongodb/conf
   ### (3).修改配置文件
    添加如下内容到 vi /usr/local/eversec/mongodb/conf/mongodb.conf
    # mongod.conf
    
    # for documentation of all options, see:
    #   http://docs.mongodb.org/manual/reference/configuration-options/
    
    # where to write logging data.
    systemLog:
      destination: file
      logAppend: true
      path: /home/mongodb/logs/mongodb.log
    
    # Where and how to store data.
    storage:
      dbPath: /home/mongodb/data/
      journal:
        enabled: true
    #  engine:
    #  mmapv1:
    #  wiredTiger:
    
    # how the process runs
    processManagement:
      fork: true  # fork and run in background
      pidFilePath: /var/run/mongodb/mongod.pid 
    # location of pidfile
    
    # network interfaces
    net:
      port: 27017
      bindIp: 127.0.0.1
    # eg: 127.0.0.1,192.168.200.221 Listen to local interface only, comment to listen on all interfaces.
    
    #security:
    
    #operationProfiling:
    
    #replication:
    
    #sharding:
    
    ## Enterprise-Only Options
    
    #auditLog:
    
    #snmp:
    注：
    修改日志文件路径，例：systemLog下的path
    修改数据文件路径，例：storage下的dbPath
    远程访问时，绑定具体IP。例：bindIp:127.0.0.1,192.168.50.228(根据服务器ip填写)
## 无认证启动
    mongod --config /usr/local/mongodb/conf/mongodb.conf  --dbpath /home/mongodb/data 
## 客户端连接
    ./mongo
    关闭：
    use admin;
    db.shutdownServer();
## 设置验证用户启动
    设置用户见下方：无认证启动，添加用户后，配置，认证启动
    security:
    Linux：authorization: enabled
    windows:mongodb.conf 配置 auth=true;
    mongod --config /usr/local/mongodb/conf/mongodb.conf  --dbpath /home/mongodb/data --auth
## 客户端连接
    ./mongo -uroot -p123456 --authenticationDatabase admin
## 查看端口占用
    netstat -lanp | grep "27017"
## mongodb设置开机启动:
	vim  /etc/rc.local
	mongod --dbpath=/usr/local/mongodb/data --logpath=/usr/local/mongodb/logs/mongodb.log --logappend --fork
## 关闭mongodb
    use admin;
    db.shutdownServer();
## 相关操作
   ### 创建数据库：
    use database;
    eg: use test;
   ###查看数据库下有那些集合：
    show collections;
   ###  删除数据库
    db.dropDatabase();
    注意：database下在有collection时才会显示
   ###  查看数据库列表
    show dbs;
   ### 创建集合
    db.createCollection("namme",[options]); 
    eg: db.createCollection("student");
    name必须要有，options可以省略
   ###  删除集合
    db.collection.drop(); eg:db.student.drop();
   ### 插入命令
    db.collection.insert(document);
    document:json格式
    eg: 
    db.student.insert({"name":"张三","age":24});
   ### 更新
    db.collection.update({*},{*});
    eg:
    db.student.update({"name":"zhangsan"},{"name":"李四","age":20})
   ### 更新2
    db.student.update({"name":"zhangsan"},{$set:{"name":"李四","age":40}},{multi:true});
    $set：修改器相比直接update,直接update:先删除，在插入,$set:做到了真正的更新，可以对指定字段进行更新
    multi: false表示只修改第一个匹配的文档，true：表示更新所有匹配的文档
   ### 删除
    db.student.remove(); // 全部删除
    db.student.remove({"name":"张三"});// 匹配删除
   ### 查询
    db.student.find();//查询全部
    db.student.find({"name":"张三"});//匹配查询
    db.student.find({"name":"张三"},{name:1,age:1,_id:0});//匹配查询,过滤域显示
   ### 用户创建
    用户创建是和数据库对应的，admin数据库
    eg:
    use admin;
    db.createUser(
            {
            user:"root",
            pwd:"123456",
            roles:[{role:"root",db:"admin"}]
            }
    );
    结果:
    Successfully added user: {
            "user" : "root",
            "roles" : [
                    {
                            "role" : "root",
                            "db" : "admin"
                    }
            ]
    }
    use admin;
    db.createUser(
        {
        user:"test",
        pwd:"123456",
        roles:[{role:"root",db:"admin"}]
        }
    );
   ### 查看用户
    use admin;
    show users;
   ### 删除用户
    db.dropUser("test");
   ### 修改用户角色
    db.updateUser("test",{roles:[{role:"readWriteAnyDatabase",db:"admin"}]});
    结果：
    {
            "_id" : "admin.test",
            "user" : "test",
            "db" : "admin",
            "roles" : [
                    {
                            "role" : "readWriteAnyDatabase",
                            "db" : "admin"
                    }
            ]
    }
   ### 修改用户密码:
    db.changeUserPassword("test","newPass");
    然后使用新密码登录进行验证。