---
title: crond定时任务二
date: 2018-07-12 22:37:22
updated: 2018-07-12 23:07:55
tags: crontab
categories: linux
---
## CentOS安装crontab:
    yum install crontabs
## crond服务操作
    service crond start     //启动服务
    service crond stop      //关闭服务
    service crond restart   //重启服务
    service crond reload    //重新载入配置
    查看crontab服务状态：service crond status
    手动启动crontab服务：service crond start
## 查看crontab服务是否已设置为开机启动
    ntsysv
    设置开机启动
    chkconfig crond on
## 定时脚本编写
    1)、在命令行输入: crontab -e 然后添加相应的任务，wq存盘退出
      eg: crontab -e 
          */1 * * * *  /home/test.sh	每过一分钟执行一次
          0 4 * * *  /home/test.sh  每天凌晨四点执行一次
    2)、直接编辑/etc/crontab 文件
    vi /etc/crontab，添加相应的任务(这种方式不推荐)
## 查看当前用户的定时执行任务
    crontab -l
    crontab -u root -l 查看指定用户的任务
## 文件格式
     Minute Hour Day Month DayofWeek CommandPath
## 参数说明
    Minute：每个小时的第几分钟执行该任务；取值范围0-59
    Hour：每天的第几个小时执行该任务；取值范围0-23
    Day：每月的第几天执行该任务；取值范围1-31
    Month：每年的第几个月执行该任务；取值范围1-12
    DayOfWeek：每周的第几天执行该任务；取值范围0-6，0表示周末
    CommandPath：指定要执行的程序路径
## 时间格式
    * ：表示任意的时刻；如小时位 * 则表示每个小时
    n ：表示特定的时刻；如小时位 5 就表示5时
    n,m ：表示特定的几个时刻；如小时位 1,10 就表示1时和10时
    n－m ：表示一个时间段；如小时位 1-5 就表示1到5点
    */n : 表示每隔多少个时间单位执行一次；如小时位 */1 就表示每隔1个小时执行一次命令，也可以写成 1-23/1
## 调度示例
    * 1 * * * /opt/script/backup.sh ：从1:0到1:59 每隔1分钟 执行
    15 05 * * * /opt/script/backup.sh ：05:15 执行
    */10 * * * * /opt/script/backup.sh ：每隔10分 执行
    0 17 * * 1 /opt/script/backup.sh ：每周一的 17:00 执行
    2 8-20/3 * * * /opt/script/backup.sh  8:02,11:02,14:02,17:02,20:02 执行
    30 21 * * * /etc/init.d/nginx restart            //每晚的21:30重启 nginx。
    45 4 1,10,22 * * /etc/init.d/nginx restart        //每月1、 10、22日的4 : 45重启nginx。
    10 1 * * 6,0 /etc/init.d/nginx restart            //每周六、周日的1 : 10重启nginx。
    0,30 18-23 * * * /etc/init.d/nginx restart        //每天18 : 00至23 : 00之间每隔30分钟重启nginx。
    0 23 * * 6 /etc/init.d/nginx restart              //每星期六的11 : 00 pm重启nginx。
    * */1 * * * /etc/init.d/nginx restart            //每一小时重启nginx
    * 23-7/1 * * * /etc/init.d/nginx restart          //晚上11点到早上7点之间，每 隔一小时重启nginx
    0 11 4 * mon-wed /etc/init.d/nginx restart        //每月的4号与每周一到周三 的11点重启nginx
    0 4 1 jan * /etc/init.d/nginx restart            //一月一号的4点重启nginx
    */30 * * * * /usr/sbin/ntpdate 210.72.145.20      //每半小时同步一下时间