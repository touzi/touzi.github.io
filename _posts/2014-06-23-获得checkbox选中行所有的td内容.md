---
date: 2014/6/23 20:37
layout: post
title: 获得checkbox选中行所有的td内容
thread: 1
categories: 前端技术
tags: html table tr td

---

###获得checkbox选中行所有的td内容

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
	<html>
	<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
    <title>Untitled</title>
    <script>
        function test(o) {
            if (!o.checked) {
                return;
            }
            var tr = o.parentNode.parentNode;
            var tds = tr.cells;
            var str = "you click ";
            for(var i = 0;i < tds.length;i++){
                if (i < 3) {
                    str = str + tds[i].innerHTML + "--";
                }
            }
            alert(str);
        }
    </script>
	</head>
	<body>
	<table width="60%" cellspacing="0" cellpadding="0" align="left" border="1" bordercolordark="#ffffff" bordercolorlight="#B3B5B4" >
    <tbody id="tabstore1">
        <tr>
            <td>b1</td>
            <td>b2</td>
            <td>b3</td>
            <td><input type="checkbox" onclick="test(this);"/></td>
        </tr>
        <tr>
            <td>c1</td>
            <td>c2</td>
            <td>c3</td>
            <td><input type="checkbox" onclick="test(this);"/></td>
        </tr>
    </tbody>
	</table>
	</body>
	</html>


--------

###判断每个table中每列的值是否相同

	function tijiao(){
	 var selectedid=document.all("checkId"); //取到select 的
	 var kh = new Array();
	 var zh = new Array();
	 var j = 0;
	 if (typeof(selectedid.length)!="undefined"){
	 for(var i=0;i<selectedid.length;i++)
	 {
     if(selectedid[i].checked){
      var tr = selectedid[i].parentNode.parentNode;
          var tds = tr.cells;
          kh[j] = tds[2].innerHTML;
          zh[j] = tds[5].innerHTML;
          j++;
          if(j>1){
                 //alert(kh[j-2]+"---"+kh[j-1]);
                 if(kh[j-2] != kh[j-1] || zh[j-2] != zh[j-1]){
                  alert("客户或者账户不相同!");
                 }
             }
    }
	 }
	}

----

end

2014/6/23 22:44:09 