---
date: 2014/6/15 20:15  
layout: post
title: python requests的安装
thread: 1
categories: python
tags: python requests

---

#python requests的安装
----

> 强烈推荐！requests官方文档已有了中文版，请见http://cn.python-requests.org/en/latest/。

![requests](http://tblogmarkdown.qiniudn.com/requesls.png)

requests是python的一个HTTP客户端库，跟urllib，urllib2类似，那为什么要用requests而不用urllib2呢？官方文档中是这样说明的：

> python的标准库urllib2提供了大部分需要的HTTP功能，但是API太逆天了，一个简单的功能就需要一大堆代码。


我也看了下requests的文档，确实很简单，适合我这种懒人。下面就是一些简单指南。

安装很简单，我是win系统，就在[这里](http://docs.python-requests.org/en/latest/user/install/#install)下载了安装包（网页中download the zipball处链接），然后$ python setup.py install就装好了。

当然，有easy_install或pip的朋友可以直接使用：easy_install requests或者pip install requests来安装。

至于linux用户，[这个页面](http://docs.python-requests.org/en/latest/user/install.html#install)还有其他安装方法。

测试：在IDLE中输入import requests，如果没提示错误，那说明已经安装成功了！

----------

1. 官方文档

	requests的具体安装过程请看：http://docs.python-requests.org/en/latest/user/	install.html#install
	requests的官方指南文档：http://docs.python-requests.org/en/latest/user/	quickstart.html
	requests的高级指南文档：http://docs.python-requests.org/en/latest/user/	advanced.html#advanced
2. 本文内容部分翻译自官方文档，部分自己归纳。
3. 大多数用的IDLE格式，累死了，下次直接用编辑器格式，这样更符合我的习惯。
4. 还是那句话，有问题留言或email。
5. 图注：requests官方文档上的一只老鳖。


------

end

2014年6月15日20:27