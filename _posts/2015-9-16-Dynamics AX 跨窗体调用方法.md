---
date: 2015/9/16 10:22:25 
layout: post
title: Dynamics AX 跨窗体调用方法
thread: 1
categories: AX
tags: 开发

---

### Dynamics AX 跨窗体调用方法

### 问题: AX中子窗体关闭后刷新父窗体

### 解决方法: 

1. 通过实例化父窗体,然后调用父窗体中的刷新方法
2. element.args()获取父窗体
3. formHasMethod(FormRun fr, IdentifierName methodName)方法的使用

#### 核心代码如下:

    static void TestRefresh(Args _args)
	{
		Object  formRunObject;
		if(element.args().caller() && formHasMethod(element.args().caller(), identifierStr(你在父窗体中写的刷新方法名称)))
    	{
        	formRunObject = element.args().caller();
        	formRunObject.你在父窗体中写的方法名;
    	}
	}

这里我们还需要在父窗体中编写一个刷新窗体的方法,具体代码如下

	public void refresh()
	{
    	窗体名称_ds.research();
	}



-----------------------------

end

9/16/2015