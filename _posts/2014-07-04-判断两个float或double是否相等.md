---
date: 2014/7/4 10:27:25 
layout: post
title: 判断两个float或double 是否相等
thread: 1
categories: JAVA
tags: JAVA 数据类型 float double

---

不能直接if(a==b)

而是要equal(a,b)

equal 函数自己写

	bool equal(double num1,double num2)
	{
	if((num1-num2>-0.000001)&&(num1-num2)<0.000001)return true;
	else return false;
	}

-------

end

2014/7/4 10:28:38 