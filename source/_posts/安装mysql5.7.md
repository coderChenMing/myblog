---
title: 安装mysql
date: 2018-10-07 16:01:28
tags: mysql
categories: database
---
## 下载mysql-5.7安装包
    centos6下载:
    wget ftp://192.168.205.11/Third_party/linux/MySQL-5.7.23.zip
   <img src="/images/mysql5.7/getMysql.png" width="1000px" height="300px" alt="下载mysql-5.7安装包">
   
## 解压安装文件
    unzip MySQL-5.7.23.zip
## 查看是否已安装mysql
    rpm -qa | grep -i mysql
## 如已经安装则停止mysql进程
    service mysqld stop
##  卸载历史安装
    rpm -e  + 已安装

##  安装5.7版本
    rpm -ivh mysql-community-common-5.7.23-1.el6.x86_64.rpm       
    rpm -ivh mysql-community-libs-5.7.23-1.el6.x86_64.rpm
    rpm -ivh mysql-community-client-5.7.23-1.el6.x86_64.rpm
    rpm -ivh mysql-community-server-5.7.23-1.el6.x86_64.rpm 
    rpm -ivh mysql-community-devel-5.7.23-1.el6.x86_64.rpm
    注: base9.2时需安装numactl-2.0.9-2.el6.i686.rpm
   ### 报错1
    1. mysql-community-common(x86-64) >= 5.7.9 is needed by mysql-community-libs-5.7.23-1.el6.x86_64 
   #### 解决办法
    清除yum里所有mysql依赖包
    rpm -qa|grep mysql
    yum remove mysql-libs
   ### 报错2
       error: Failed dependencies:
       libaio.so.1()(64bit) is needed by mysql-community-server-5.7.23-1.el6.x86_64
       libaio.so.1(LIBAIO_0.1)(64bit) is needed by mysql-community-server-5.7.23-1.el6.x86_64
       libaio.so.1(LIBAIO_0.4)(64bit) is needed by mysql-community-server-5.7.23-1.el6.x86_64
       libsasl2.so.2()(64bit) is needed by mysql-community-server-5.7.23-1.el6.x86_64
   #### 解决办法 
    yum install libaio
   ### 再报错
    error: Failed dependencies:
        libsasl2.so.2()(64bit) is needed by mysql-community-server-5.7.23-1.el6.x86_64
## 修改数据表存储路径为/home/mysql/data
    a.清除/home/mysql/data目录下文件
      rm -rf /home/mysql/data/*
    b.修改/etc/my.cnf配置文件
      vi /etc/my.cnf
      datadir=/var/lib/mysql修改为datadir=/home/mysql/data
      socket=/var/lib/mysql/mysql.sock修改为socket=/home/mysql/data/mysql.sock
      按Esc键，输入:wq，按回车键保存退出
    c.创建mysql.sock软链
      ln -s /home/mysql/data/mysql.sock /var/lib/mysql/mysql.sock
## 启动mysql
    service mysqld start
## 无密登陆设置密码
    mysqld_safe --skip-grant-tables
   ### 为了安全可以这样禁止远程连接：
    mysqld_safe--skip-grant-tables--skip-networking
    现在可以无密登录
    mysql -uroot -p 两次回车即可登录
   ### 无密登录后设置新密码
    use mysql;
    update  mysql.user  set  authentication_string=password("新密码") where  user="root"  and host="localhost";
    密码等级默认MEDIUM,所以不能设置简单密码
    update  mysql.user  set  authentication_string=password("QHDcm@1991") where  user="root"  and host="localhost";
    update  mysql.user  set  password_expired=password("新密码") where  user="root"  and host="localhost";
    flush  privileges;  //更新
    quit;
    说明：select password("123456"):password(*):函数加密
    退出后使用新密码登录
    mysql -u root -p 新密码
    
    常见错误：
    ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement
    解决办法
    eg：alter user  'root'@'localhost' identified by'123456';
    ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
    eg:alter user  'root'@'localhost' identified by'QHDcm@1991';
##  符合标准的新密码登录后为了能够改成简单密码，需要修改一些参数
    mysql> set global validate_password_policy=0;
    Query OK, 0 rows affected (0.05 sec)
    
    mysql> set global validate_password_mixed_case_count=0;
    Query OK, 0 rows affected (0.00 sec)
     
    mysql> set global validate_password_number_count=3;
    Query OK, 0 rows affected (0.00 sec)
     
    mysql> set global validate_password_special_char_count=0;
    Query OK, 0 rows affected (0.00 sec)
     
    mysql> set global validate_password_length=3;
    Query OK, 0 rows affected (0.00 sec)
     
    mysql> SHOW VARIABLES LIKE 'validate_password%';
    +--------------------------------------+-------+
    | Variable_name                        | Value |
    +--------------------------------------+-------+
    | validate_password_dictionary_file    |       |
    | validate_password_length             | 3     |
    | validate_password_mixed_case_count   | 0     |
    | validate_password_number_count       | 3     |
    | validate_password_policy             | LOW   |
    | validate_password_special_char_count | 0     |
    +--------------------------------------+-------+
    6 rows in set (0.00 sec)
    
    validate_password_dictionary_file
    插件用于验证密码强度的字典文件路径。
    
    validate_password_length
    密码最小长度，参数默认为8，它有最小值的限制，最小值为：validate_password_number_count + validate_password_special_char_count + (2 * validate_password_mixed_case_count)
    
    validate_password_mixed_case_count
    密码至少要包含的小写字母个数和大写字母个数。
    
    validate_password_number_count
    密码至少要包含的数字个数。
    
    validate_password_policy
    密码强度检查等级，0/LOW、1/MEDIUM、2/STRONG。有以下取值：
    Policy                 Tests Performed                                                                                                        
    0 or LOW               Length                                                                                                                      
    1 or MEDIUM         Length; numeric, lowercase/uppercase, and special characters                             
    2 or STRONG        Length; numeric, lowercase/uppercase, and special characters; dictionary file      
    默认是1，即MEDIUM，所以刚开始设置的密码必须符合长度，且必须含有数字，小写或大写字母，特殊字符。  
    validate_password_special_char_count
    密码至少要包含的特殊字符数。
   ### 在my.cnf中配置永久生效
    validate_password_policy=0
    validate_password_length=3
    validate_password_special_char_count=0
    validate_password_mixed_case_count=0
   ### 修改为简单密码
    update  mysql.user  set  authentication_string=password("123456") where  user="root"  and host="localhost";
## 用户授权
    grant all privileges on *.* to root@'localhost' identified by '123456' with grant option;  
    flush privileges;
    quit;
    CREATE USER 'javacui'@'localhost' IDENTIFIED BY '123456'; 
    CREATE USER 'javacui'@'172.20.0.0/255.255.0.0' IDENDIFIED BY '123456'; 
    CREATE USER 'javacui'@'%' IDENTIFIED BY '123456'; 
    CREATE USER 'root'@'%' IDENTIFIED BY '123456'; 
    CREATE USER 'mysql'@'%' IDENTIFIED BY '123456'; 
    CREATE USER 'javacui'@'%' IDENTIFIED BY ''; 
    CREATE USER 'javacui'@'%';
    CREATE USER 'mysql'@'%';
    CREATE USER 'root'@'%';
    ALTER USER 'root'@'%' PASSWORD EXPIRE NEVER;
    databasename –  数据库名
    tablename - 表名,如果要授予该用户对所有数据库和表的相应操作权限则可用* 表示, 如*.*
    GRANT privileges ON databasename.tablename TO 'username'@'host';
    GRANT SELECT, INSERT ON test.user TO 'javacui'@'%'; 
    GRANT ALL ON *.* TO 'javacui'@'%';
    GRANT ALL ON *.* TO 'root'@'%';
    GRANT ALL ON *.* TO 'mysql'@'%';
## 设置与更改用户密码
    SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
    撤销用户权限
    REVOKE privilege ON databasename.tablename FROM 'username'@'host';
    privilege, databasename, tablename – 同授权部分
    REVOKE SELECT ON *.* FROM 'javacui'@'%';
    删除用户
    DROP USER ‘username’@'host’;
    操作后切记刷新数据库
    flush privileges;
## 安装好的mysql,关机后就起不来了
    查看了一下MySQL的启动日志`less /var/log/mysqld.log` 
    错误日志如下：
    [ERROR] Can’t start server: can’t create PID filepath: No such file or directory
    
    错误原因
    一般是由于服务器强制关机导致pid文件丢失。
    
    解决办法
    在/etc/my.cnf 中查看pid-file的位置
    
    pid-file=/var/run/mysql/mysqld.pid
    创建对应的目录并修改权限
    
    mkdir -p /var/run/mysql
    touch /var/run/mysql/mysqld.pid
    
    因为MySQL启动是用mysql这个用户运行的,我们之前用的是root用户创建的文件夹,那么mysql用户是没有权限的,所以我们需要给mysql用户授权
    chown mysql.mysql /var/run/mysql/mysqld.pid
    再启动MySQL应该就没问题了。
    service mysqld start
    Starting mysqld (via systemctl): [ 确定 ]
    到此, 启动成功了
    然而并没有结束
    再次重启动虚拟机, mysql服务又又又TM起不起来了, 依然是老问题, 但是每次都去配置一下, 也不是个事.
    虽然可以写个脚本解决, 但是感觉还是不方便.遂~ 我把pid文件的位置换了个地方.
    这里需要说明一下, 默认mysqld.pid的路径是`/var/run/mysqld/mysqld.pid` 这个路径是放在内存中的, 所以每次关机了就会删除.
    我把这个文件换到了这个路径下面`/var/lib/mysql/mysqld.pid`,需要做的修改如下:
    
    ### 1. 修改配置文件
    修改`/etc/my.cnf`里面的pid-file值为这样:
    pid-file=/var/lib/mysql/mysqld.pid
    
    ### 2. 修改启动文件
    修改`/etc/init.d/mysqld`文件里面的pid-file为这样:(使用vim命令, 然后找到这一行修改)
    get_mysql_option pid-file "/var/lib/mysql/mysqld.pid" mysqld mysqld_safe
    
    ### 3. 重新加载启动文件
    systemctl daemon-reload
    
    ### 4. 重启MySQL
    service mysqld start
    
    ### 5. 将MySQL加入开机启动
    如果你的mysql没有开启启动,可以使用这个命令加入:
    chkconfig mysqld on
    Can't create/write to file '/home/mysql/data/is_writable' (Errcode: 13 - Permission denied)
    chown mysql.mysql -R /home/mysql/data
    vi /etc/selinux/config  改成disabled
##  数据库报错：this is incompatible with sql_mode=only_full_group_by
    select 选取分组中的列+聚合函数 from 表名称 group by 分组的列 
    从语法格式来看，是先有分组，再确定检索的列，检索的列只能在参加分组的列中选。
    ONLY_FULL_GROUP_BY的意思是：对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么这个SQL是不合法的，因为列不在GROUP BY从句中，
    也就是说查出来的列必须在group by后面出现否则就会报错，或者这个字段出现在聚合函数里面。
    
    查看mysql版本命令：select version();
    查看sql_model参数命令：
    SELECT @@GLOBAL.sql_mode;
    SELECT @@SESSION.sql_mode;
    发现：
    ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
    第一项默认开启ONLY_FULL_GROUP_BY，
    解决方法：
    1.只选择出现在group by后面的列，或者给列增加聚合函数；（不推荐）
    2.命令行输入：
    set @@GLOBAL.sql_mode='';
    set sql_mode ='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
    默认关掉ONLY_FULL_GROUP_BY！
     
    这个时候 在用工具select 一下
    SELECT @@sql_mode;
    SELECT @@GLOBAL.sql_mode;
     
    发现已经不存在ONLY_FULL_GROUP_BY ，感觉已经OK。但是如果你重启Mysql服务的话，发现ONLY_FULL_GROUP_BY还是会存在的
     
    想要彻底解决这个问题 就得去改my.ini 配置 （如果你们mysql 没有这个文件，就把my-default.ini 改成my.ini，我这个版本就是没有my.ini配置问题）
     
    在 [mysqld]和[mysql]下添加
    sql_mode =STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
    
    
    mysql数据库的中有一个环境变量sql_mode,定义了mysql应该支持的sql语法，数据校验等
    mysql5.0以上版本支持三种sql_mode模式：ANSI、TRADITIONAL和STRICT_TRANS_TABLES。
    ANSI模式：宽松模式，对插入数据进行校验，如果不符合定义类型或长度，对数据类型调整或截断保存，报warning警告。
    TRADITIONAL模式：严格模式，当向mysql数据库插入数据时，进行数据的严格校验，保证错误数据不能插入，报error错误。用于事物时，会进行事物的回滚。
    STRICT_TRANS_TABLES模式：严格模式，进行数据的严格校验，错误数据不能插入，报error错误。   
   