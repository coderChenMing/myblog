---
title: node&&npm&&nvm安装
tags: 前端
categories: nodejs
abbrlink: 1100949762
date: 2020-08-07 23:21:06
updated: 2020-08-07 23:51:01
---
## vue概念
    MVVM:将业务逻辑代码与视图代码完全分离，各司其职，方便维护，降低维护成本
    model:数据存储
    view:页面展示
    view model:业务逻辑处理，比如ajax请求,对数据加工后交给视图展示
    v-text：解决插值表达式闪烁的问题
## webpack
    1.导出方法
    2.导入
    3.打包
    4.html引用
## 为什么使用nvm
    我们一般安装的是最新版本的node,但是有时候，我们又需要使用低版本的node,在不借助第三方工具的情况下，
    我们只能卸载现有版本,安装需要的版本，这样显然很麻烦，今天就来介绍一个windows系统使用的node多版本管理工具nvm，
    全称是node.js version management,可以在多个node版本之间自由切换！
   下载:[nvm](https://github.com/coreybutler/nvm-windows/releases)

## 安装nvm
    解压.zip压缩包，双击nvm-setup.exe开始安装，下面两个步骤需要注意：
    注意：安装之前必须完全卸载已安装的node
    安装完成后，在CMD命令窗口输入nvm -v,输出如下版本号即证明安装成功

## 安装node
    查看nvm支持安装的node版本：nvm list available,使用nvm install 版本号安装指定版本node:nvm install 12.18.3
    检测node和npm是否安装成功,如下显示则为成功：node -v,npm -v

    初次使用nvm安装node之后，必须先使用nvm use 版本号切换到已安装版本的node才可以:nvm use 12.18.3

    多版本node切换:
    查看当前安装的所有node版本:nvm list,*号符表示的是当前使用的版本
    切换不同版本的node:nvm use 版本号

## 常用指令
    nvm off                     //禁用node.js版本管理(不卸载任何东西)
    nvm on                      //启用node.js版本管理
    nvm install <version>       //安装指定版本node,例如nvm install 12.18.3
    nvm uninstall <version>     //卸载指定版本node,例如nvm uninstall 12.18.3 
    nvm list                    //显示所有安装的node.js版本
    nvm list available          //显示可以安装的所有node.js的版本(windows系统)
    nvm use <version>           //切换到使用指定的nodejs版本，例如nvm use 12.18.3

## 项目环境问题解决
    切换版本之后，node_module文件正常情况需要重新构建，所以需要进入项目文件夹执行npm install；

    重新安装npm install 以后，执行npm run dev 启动项目，正常即可启动成功。
    有时会遇到有些npm包报错，需要重新rebuild一下，
    错误提示: Node Sass does not yet support your current environment:
    npm rebuild node-sass 即可解决问题。
## nodejs命令
    查看全局安装路径: npm root -g
    查看npm的基础设置: npm config ls
    查看安装目录路径: npm config get prefix
    切换npm源为淘宝源: npm config set registry https://registry.npm.taobao.org
    查看npm源: npm config get registry
    cnpm可以替代npm使用，安装cnpm:npm install -g cnpm --registry=https://registry.npm.taobao.org
    
## npm和cnpm混用的坑
    因为cnpm默认使用的是软链接，会导致npm安装后，更新了之前的cnpm包，然后之前的cnpm引入就会gg了，于是，gg了一大堆东西。
    那么我们可以发现，原因在于cnpm本身，如果我们不用它问题就解决了
    那么最简单的办法就是使用npm install <一些参数> --registry=https://registry.npm.taobao.org
    这样就很完美了，但是这样又很麻烦
    这时候可以升级下，用nrm
## 安装nrm
    npm install -g nrm
    显示使用的源地址:  nrm ls
    切换源,使用npm下载包的时候，使用cnpm的镜像源下载东西:nrm use cnpm

    了解每个源下载的速度:nrm test
    如果已经项目用了cnpm怎么办？
    方式改进: cnpm i --by=npm
 ## 全局安装webpack指定版本
    npm install webpack@3.6.0 -g