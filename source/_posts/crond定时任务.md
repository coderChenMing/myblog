---
title: crond定时任务
tags: crond
categories: linux
abbrlink: 1269898537
date: 2018-06-07 20:27:36
updated: 2018-06-07 21:21:18
---
## crond服务实现定时任务。
    cron也是在/etc/init.d/中注册从而自启的，启动后它会扫描/var/spool/cron 、/etc/cron.d目录和 /etc/anacrontab文件，扫描到的文件中定义了各种定时任务。
## cron有两个配置文件，
    一个是一个全局配置文件（/etc/crontab），是针对系统任务的；
    一组是crontab命令生成的配置文件（/var/spool/cron下的文件），是针对某个用户的.
    定时任务配置到任意一个中都可以。
## 查看用户下的定时任务
    crontab -l或cat /var/spool/cron/用户名
## 表达式解析
    min(0-59) 
    hour(0-3) 
    day of month(1-31) 
    month(1-12) 
    day of week(0-6)
    eg: 0 1 * * *
## 启动、停止、重启服务↓
    systemctl start crond.service

    systemctl stop crond.service

    systemctl restart crond.service
    
    systemctl reload crond.service  
## 清理垃圾文件
    linux是一个很能自动产生文件的系统，日志、邮件、备份等。定时删除就显的很方便。
    语句写法：
    find 对应目录 -mtime +天数 -name "文件名" -exec rm -rf {} /;
    eg：
    将/backups目录下所有10天前带"."的文件删除:
    find /backups -mtime +10 -name "*.*" -exec rm -rf {} /;
    解析:
    find：linux的查找命令，用户查找指定条件的文件
    /backups：想要进行清理的任意目录
    -mtime：标准语句写法
    ＋10：查找10天前的文件，这里用数字代表天数，＋30表示查找30天前的文件
    "*.*"：希望查找的数据类型，"*.jpg"表示查找扩展名为jpg的所有文件，"*"表示查找所有文件
    -exec：固定写法
    rm -rf：强制删除文件，包括目录
    {} /; ：固定写法，一对大括号+空格+/+;
    可以将这小语句写到一个可执行文件中，再设置cron调度执行，那就可以让系统自动去清理相关文件。
## 定时清理脚本编写
    新建一个可执行文件clear.sh
    #vi clear
    #!/bin/sh
    find /u01/backups -mtime +10 -name "*.*" -exec rm -rf {} /;
    赋权
    #chmod +x clear
    将clear.sh文件加入到系统计划任务，到点自动执行
    #crontab -e
    * 2 * * * /usr/local/bin/clear
    这里的设置是每天凌晨2点执行clear脚本进行数据清理