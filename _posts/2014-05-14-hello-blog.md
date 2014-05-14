---
date: 2004-10-04 02:44:30+00:00
layout: post
title: 第一篇blog,搭建blog空间
thread: 0
categories: 分享
tags:  blog 搭建
---
#第一篇blog,搭建blog空间
#####本博客使用jekyll+github+markdown搭建而成.在此感谢Yonsm.本blog源码地址[github](https://github.com/touzi/tblog) 直接fork就可以运行.
***
因为我没有域名所以修改了 _config.yml配置文件,在其中加入了repository: 属性,这样可以在没有域名的时候然访问直接到仓库下面.如:http://touzi.github.io/tblog/ 如果加入repository这个属性那么访问首页的时候是在http://touzi.github.io 那就访问不到你的项目了.当然有其他解决方法,比如404跳转单正确路径的首页等.
***
###关于搭建
其实搭建很简单,只要去fork项目就可以直接运行.原作者已经给了配置介绍,再次不再赘述.
我搭建这个blog总共花费了3个小时,其实没有只要搭建使用的话,20分钟搞定,我是在github上花费了大量的时间,比如Windows环境下的git等.一般我都是在macbook下面使用github.
***
###为什么搭建blog
今天(2014-05-13)无意间查看到自己的evernote的笔记已经132篇了.所以决定搭建一个blog来分享自己工作学习的收货.
***
###第一次使用markdown
我用的是马克飞象的chrome插件,这个马克飞象可以直接同步evernote.
* markdown语法非常简单,对于一般的写作我觉得万全足够用了.
* 截个图给大家看简单的语法
 ![截个图给大家看看](http://tblogmarkdown.qiniudn.com/20140513165053.jpg)
* 上面图片效果不怎么样,我用dropbox的共享功能,发现图片一直不能被插入.然后我决定用七牛了[七牛官网](https://portal.qiniu.com/signup?code=3ll21nl4v4hua)
* 直接导出.md文件上传就可以发布blog
* end

***
###总结
我完全不懂jekyll,但看到效果特别精简,非常喜欢所以就fork下来了.下面给出我这个博客的github托管地址,有兴趣的同学可以clone下来自行修改.[点击这里](https://github.com/touzi/tblog) 
