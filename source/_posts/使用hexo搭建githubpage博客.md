---
title: 使用hexo搭建githubpage博客
date: 2020-08-07 00:19:58
tags: [hexo]
categories: hexo
---
## 为什么要搞博客？
   互联网技术日新月异，如何快速又高效的提高我们的技术水平是每一个程序员都要面临的问题，像我，一天工作8-10小时，
上下班路上还要花费3到4个小时，除却这些时间，每天留给自己学习的时间不过两三个小时。大家都知道，现在行内内卷严重，
无论是出于家庭原因还是个人职业的发展，充分利用这有效的一点时间来提升自己的技术水平是十分重要的，虽然现在学习一门技术
有很多手段，比如看b站视频，某宝买点资源，或者是工资内部学习资料等等，各种方式大大降低了我们学习的困难程度，但是人类的脑子
是健忘的，所谓好记性不如烂笔头，将学习成果通过博客的方式来输出，我觉得是非常有利于我们自己的一件事。
    好处呢，大概有以下几点：
        1.加深印象。这点很有体会，某个技术点学起来没什么难度，但是你又不经常用，过了一段时间，忘了，再想拿起来，还得四处
    找资料，总结，重复成本很高。
        2.方便查找。同样，将学习成果按照自己的习惯分类，当你需要用的时候，很容易查找，很方便.
        3.培养总结的习惯。很好的培养自己思考和总结的能力，说不定哪天还能创造点东西，哈哈。
        4.交流进步。众人拾柴火焰高，一人一个想法，一千个人交流，每个人将会有一千个想法，棒棒哒.
## 什么是hexo?
 [Hexo](https://hexo.io/zh-cn/)是一个快速、简洁且高效的博客框架。
 Hexo使用Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
 当我们在遇到一个新东西的时候，进入其官网能帮助我们解决很多问题。关于什么是Hexo？
 还有一些关于Hexo的问题，可以进入Hexo官网进行查看，相信你会收益良多.
 
 ## 使用hexo搭建博客
   安装Hexo相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
   Node.js
   Git
   npm/cnpm 如果npm运行出错就安装cnpm国内镜像
   如果没有上面三个必须的环境，那就需要逐个安装了，网上很多方法可以安装，自行google
   ### 安装Hexo
   打开Git Bash Here输入:
   npm install -g hexo-cli/cnpm install -g hexo-cli
   检查是否安装Hexo完成,查询是否成功:
   hexo -V
## 搭建博客
   个人机器创建存储博客的文件夹或者目录
   执行：
   hexo init \<folder>
   cd \<folder>
   npm/cnpm install
   说明：\<folder>是博客项目所在目录
   eg: 
   1.hexo init myblog
   2.cd myblog
   3.npm/cnpm install
   以上执行完毕，就有了一个简单的博客，访问：localhost:4000查看
## 上传github发布
   1.在github上创建后缀名为github.io的repository
   eg: username.github.io
   2.修改博客目录下的_config.yml文件:
   deploy:
     type: git
     repository: https://github.com/username/username.github.io.git #复制过来,username按照你自己自定义替换
     branch: master
   url: http://username.github.io
   3.部署
    3.1 部署之前在博客目录路径gitbash执行:npm/cnpm install hexo-deployer-git --save
    3.2 每次提交时必须执行这三个命令:
        hexo clean
        hexo generate
        hexo deploy
        或者简写方式
        hexo clean
        hexo g
        hexo d
    3.3 启动博客查看
        hexo s  ,访问localhost:4000查看
## Hexo 项目目录结构说明
   deploy：执行hexo deploy命令部署到GitHub上的内容目录
   public：执行hexo generate命令，输出的静态网页内容目录
   scaffolds：layout模板文件目录，其中的md文件可以添加编辑
   scripts：扩展脚本目录，这里可以自定义一些javascript脚本
   source：文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，404文件、favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
        drafts：草稿文章
        posts：发布文章
   themes：主题文件目录
   _config.yml：全局配置文件，大多数的设置都在这里
   package.json：应用程序数据，指明hexo的版本等信息，类似于一般软件中的关于按钮
   
   