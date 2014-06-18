---
date: 2014/6/18 21:39:50 
layout: post
title: 使用Android API最佳实践
thread: 1
categories: 移动开发
tags: Android API

---

#使用Android API最佳实践

----

##做什么，如何做？

现在，我们要依次学习使用Retrofit、OKHttp和GSON，简单快速的集成REST API。使用这个组合，我们需要从Twitch.tv下载并解析一些数据。跟着下面的步骤可以在几分钟内，不用写繁琐的模板代码，完成大部分的REST API集成。

###Retrofit

[Retrofit](https://github.com/square/retrofit)简化了从Web API下载数据，解析成普通的Java对象（POJO）。例如，要从Github 上下载用户仓库的信息，你只需要编写下面的几行：

	@GET("/users/{user}/repos")
	List listRepos(@Path("user") String user);

另外，你需要创建仓库信息类和数据类型。这些代码也可以自动生成，下面会介绍如何自动生成。

整个过程很简单，类似发送一次有参数的请求或发送POST或HEAD。如何连接不同类型的API，请查看[说明文当](http://square.github.io/retrofit/)。

Retrofit的特性之一可以将处理逻辑添加到请求和响应中。你可以添加数据到http请求头部，也可以拦截验证失败的响应重定向到登录界面。

###OKHttp

[OKHttp](http://square.github.io/okhttp/)是Android版Http客户端。非常高效，支持SPDY、连接池、GZIP和 HTTP 缓存。默认情况下，OKHttp会自动处理常见的网络问题，像二次连接、SSL的握手问题。如果你的应用程序中集成了OKHttp，Retrofit默认会使用OKHttp处理其他网络层请求。

###GSON

[GSON](https://code.google.com/p/google-gson/)是将JSON解析成POJO的Java库。GSON也可以将POJO解析成JSON。在Android中，数据对象存储在SharePreference更加方便。

要使用GSON，首先需要创建相应的POJO数据，再用GSON解析为POJO对象。解析过程简单且非常高效。需要了解如何创建可以被GSON解析的POJO对象，请查看[说明文档](https://sites.google.com/site/gson/gson-user-guide)。Retrofit使用GSON解析JSON数据。

##开始Coding

###添加库文件到工程


1. 下载[Retrofit](https://github.com/square/retrofit)、[OKHttp](https://github.com/square/okhttp)、[GSON](https://code.google.com/p/google-gson/)库文件。
2. 逐个添加jar文件到你的工程中。
3. 如果使用Android Studio，可以使用gradle同步这个工程。

###查找或者编写API

你可能已有一份API，如果你还在寻找API目录，我推荐[ProgrammableWeb](http://www.programmableweb.com/)。在这个教程中，我们会解析Twitch.Tv的数据流。请求格式请参考[说明手册](http://www.justin.tv/p/rest_api_stream_list)。Twicht.tv请求数据流的JSON格式：
[http://api.justin.tv/api/stream/list.json](http://api.justin.tv/api/stream/list.json)

##展示输出

展示一些API返回的数据，下面的示例是由于是一个GET请求，只能在浏览器中运行，返回数据如下：

	[{"broadcast_part": 4, "featured": true, "channel_subscription": true, "audio_codec": "uncompressed", "id": "6640712464", "category": "gaming", "title": "Fnatic xPeke, Normals(ranked down) on smurf", "geo": "DE", "video_height": 1080, "site_count": 8014, "embed_enabled": true, "channel": {"subcategory": null, "producer": true, "image_url_huge": "http://static-cdn.jtvnw.net/jtv_user_pictures/xpeke-profile_image-a182a5fe5a8f239b-600x600.jpeg", "timezone": "Europe/Madrid", "screen_cap_url_huge": “http://static

###创建POJO

这部分很有趣，用我们获取到的数据自动创建对应的POJO。使用[jsonschema2pojo](http://www.jsonschema2pojo.org/)，导入包名、类名和JSON数据，保存为私有类型。示例中展示的构造器无法使用，因为JSON数据的根元素是个数组，不是对象。所以我只贴出了数组的第一个元素。展示相关的图片示例。![androidapi](http://tblogmarkdown.qiniudn.com/androidAPI.png)

###集成POJOs

将自动产生的POJOs粘贴到工程中就可以了。在我的示例工程中，他们在models包中。

###使用Retrofit下载（解析）API

###创建REST Adapter

创建Adapter，类似设置endPoint。

	RestAdapter restAdapter = new RestAdapter.Builder()
	.setEndpoint("http://api.justin.tv/api")
	.build();

###定义API接口

为需要连接的endPoint定义接口。下面示例中，使用limit和offset，这两个参数用来控制请求数据位置和大小。详细说明请参考[API文档](http://www.justin.tv/p/rest_api_stream_list)。

	public interface TwitchTvApiInterface {
	@GET("/stream/list.json")
	void getStreams(@Query("limit") int limit, @Query("offset") int offset, Callback<List> callback);}

你可能会注意到，我们期望返回的是一组JustinTvStreamData对象，也就是我们刚才自动产生的POJO。关于如何定义这个接口的更多信息，请参考[Retrofit说明文档](http://square.github.io/retrofit/)。

###创建Twitch.tv 服务

现在我们已经建立了endPoint，定义了需要的接口。下面需要创建Twitch.TV服务，发送请求。

	TwitchTvApiInterface twitchTvService = restAdapter.create(TwitchTvApiInterface.class);

###使用API

发送API请求十分简单，只需要使用刚才创建的服务即可。

	twitchTvService.getStreams(ITEMS_PER_PAGE, pageNumber * ITEMS_PER_PAGE, new Callback<List>() {
	@Override
	public void success(List justinTvStreamData, Response response) {
	consumeApiData(justinTvStreamData);
	}
	@Override
	public void failure(RetrofitError retrofitError) {
	consumeApiData(null);
	}});

这里有一点需要注意，Retrofit会在后台线程下载并解析API数据，根据结果不同（成功或失败）发送到UI线程。Retrofit也支持在后台自动下载（这里没有显示）。

###数据处理趣事

现在我们用POJO数据做一些有趣的事情。在这个Demo中，展示了Twitch.Tv频道的图片和描述，使用[Picasso Library](https://github.com/square/picasso) 下载缓存图片。![androidapi2](http://tblogmarkdown.qiniudn.com/androidapi2.png)

##参考代码

[示例代码下载](https://github.com/MeetMe/TwitchTvClient)。

----------
转载自:[http://blog.jobbole.com/65170/](http://blog.jobbole.com/65170/)


------------
end

2014/6/18 21:54:55 