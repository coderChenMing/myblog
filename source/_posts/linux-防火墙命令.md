---
title: linux 防火墙命令
tags: 防火墙
categories: linux
abbrlink: 3937382499
date: 2018-03-08 23:08:01
updated: 2018-03-08 23:51:07
---
## CentOs6操作防火墙:
    配置文件:/etc/sysconfig/iptables
    开启某个端口号有两种方式: 一种是命令方式，一种是修改配置文件方式
    查看防火墙状态: service iptables status
    开启防火墙(重启后永久生效): chkconfig iptables on
    关闭防火墙(重启后永久生效): chkconfig iptables off
    开启防火墙(即时生效，重启后失效): service iptables start
    关闭防火墙(即时生效，重启后失效): service iptables stop
    重启防火墙: service iptables restartd
    查看开启的端口号: service iptables status

   ### 开启某个端口号
    开放80端口
    iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
    保存开启的端口号
    service iptables save
    重新启动防火墙
    service iptables restart

   ### 开启某个范围的端口号
    开启 18881~65534 端口
    iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 18881:65534 -j ACCEPT
    保存开启的端口号
    service iptables save
    重新启动防火墙
    service iptables restart

   ### 通过修改配置文件开启端口号
    开放80端口
    vi /etc/sysconfig/iptables
    -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
    启动防火墙
    service iptables restart

    参数说明：
    –A 参数就看成是添加一条规则
    –p 指定是什么协议，我们常用的tcp 协议，当然也有udp，例如53端口的DNS
    –dport 就是目标端口，当数据从外部进入服务器为目标端口
    –j 就是指定是 ACCEPT -接收 或者 DROP 不接收
    查看防火墙对外开放了哪些端口
    iptables -L -n
## CentOs7操作防火墙
    Centos7默认安装了firewalld，如果没有安装的话，可以使用 yum install firewalld firewalld-config进行安装。
    启动防火墙: systemctl start firewalld
    禁用防火墙: systemctl stop firewalld
    设置开机启动: systemctl enable firewalld
    停止并禁用开机启动: sytemctl disable firewalld
## 查看Linux发行版本
    uname -a
    cat /etc/redhat-release
