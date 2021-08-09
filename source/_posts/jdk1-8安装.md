---
title: jdk1.8安装
date: 2018-04-07 20:52:29
updated: 2018-04-07 21:36:15
tags: jdk
categories: java
---
## 下载jdk1.8安装包
    wget ftp://192.168.205.11/Third_party/linux/jdk-8u162-linux-x64.rpm
## 查看已安装jdk
    rpm -qa|grep jdk
   <img src="/images/install_jdk/look.png" width="900px" height="100px" alt="查看已安装jdk">
   
   ### 若已安装jdk，但其版本不是1.8.0_162，则需将其卸载
    rpm -e + 查到已安装jdk资源
## 安装jdk
    rpm -ivh jdk-8u162-linux-x64.rpm
   ![安装jdk](/images/install_jdk/install.png)
## 配置环境变量
    vi /etc/profile
   ### 在文件末尾添加如下内容
    export JAVA_HOME=/usr/java/jdk1.8.0_162
    export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH
    export CLASSPATH=.:${JAVA_HOME}/lib:${JAVA_HOME}/jre/lib:$CLASSPATH
   ### (1).立即生效：
    source /etc/profile
   ### (2).再次进入终端生效
    vi /root/.bashrc
    添加source /etc/profile
 ## 验证
    java -version
   <img src="/images/install_jdk/verify.png" width="900px" height="150px" alt="验证已安装jdk">