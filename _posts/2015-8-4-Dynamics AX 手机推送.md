---
date: 2015/8/4 20:27:25 
layout: post
title: Dynamics AX 中实现手机推送
thread: 1
categories: AX
tags: 开发

---

### Dynamics AX 中实现手机推送

### 问题: AX中实现收款之后消息推送到相关人员手机上

### 解决方法: 

1. 在需求访问量不大的情况下可以使用第三方的推送服务,也可自建push推送服务器
2. RetailCommonWebAPI的使用
3. 思路:收款之后触发方法,将消息推送到push服务器,由push服务器将消息推送到手机

#### active中代码如下:

    static void TestPush(Args _args)
	{
		    RetailWebResponse response;
		    str rawResponse,basic,data,json;
		    RetailCommonWebAPI webApi = RetailCommonWebAPI::construct();
		    basic = "Authorization: Basic Nzc4OTUzZDY0MGFiNDIxNzQ2YjZmYTQzOmQ4ZmE3NTA1NjE0YjRiOWQ1OTkxNWY0ZA==";
		    json = '{"alert":"有一个新的收款单","title":"京铁列服","builder_id":1,"extras" : {"amount":"201,321.00","trainNum":"G21","crew":"高铁乘务一组","comment":"交款正确","date":"2015/7/25"}}';
		    data = '{"platform": "all","audience": "all","notification" :{"android" :'+json+'}}';
		    response = webApi.makePostRequest("https://api.jpush.cn/v3/push",data,basic,"application/json");
		    rawResponse = response.parmData();
		    info(strFmt("Element name: %1",rawResponse));
	}

主要使用到了RetailCommonWebAPI 类中的makePostRequest方法,通过这个方法可以实现JPush的API调用,我们可以将自己定义的内容放在json中,用一个json类来规范代码.

####客户端

这边我用的是JPush提供自动生成的android的demo改的,主要修改了消息接受之后的内容展示,这里就不贴上代码了,很简单的一个json解析.

这里因为是演示DEMO所以就凑合着用了.


-----------------------------

end

8/4/2015