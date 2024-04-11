{}
date: 2024-04-10 13:49:47
---
# windows应急响应

命令
```
netstat -ano | findstr "ip/端⼝"
Get-NetTCPConnection
Get-NetUDPEndpoint
net session //查询当前系统登录情况
```
已知恶意进程名称，更好的方式是搜索文件关联的句柄
win+r eventvwr.msc 打开时间查看器
登录成功日志编号为4624，失败为4625
根据具体服务查询相对应的日志文件，看有没有发生爆破行为
查询有无添加用户，隐藏用户admin$
内网机器一直访问某台机器某一个端口是被搭建端口相应服务隧道
```工具
DNS隧道 iodine，CS等等
ICMP pingtunnel
TCP frp，iox，nc等等
UDP ncat，openvpn，iodine等等
```
工具
任务管理器：查看文件sessionID，路径，占用内存进程
<b>everything：搜索一些恶意软件名称和使用恶意软件产生的子文件</b>
procexp：查看程序调用dll
dumpbin：查看程序导入表，看有没有调用敏感的windowsAPI
<b>火绒剑：扫描钩子（进程模块钩子、内核模块钩子、驱动对象钩子、文件驱动钩子、键盘驱动钩子），查看启动项，注册表，网络（软件外连地址）</b>
OpenArk：查看句柄，内存，网络连接，扫描文件，保护进程。<b>可以暂停进程</b>
system informer：跟上面差不多

# linux应急响应






# 中间件漏洞
iis
```
远程代码执行
解析漏洞   目录以.asp结尾    xxx.asp;1.jpg
```

tomcat
```
war包后门部署
远程代码执行
```

apache
```
解析漏洞    1.php%0a换行解析   1.php.xxx 未知后缀解析
SSI远程命令执行
路径穿越
```

nginx
```
解析漏洞    1.jpg/.php
目录穿越 （nginx配置别名的时候alias的时候，忘记加/  将造成目录穿越漏洞）
CRLF注入 (Nginx会将$uri进行解码，导致传入%0a%0d即可引入换行符，造成CRLF注入漏洞)
```

jboss
```
反序列化
war后门部署
```

weblogic
```
原理：xml反序列化，这是wls security组件对外提供的webserver页面，通过xmlDecoder功能来解析用户的xml数据导致的任意字符串被当做代码去执行
特征：服务器开放7001端口  传递xml数到wls-wsat  数据包内容有bash或者dnslog字段。
```

# webshell流量特征
菜刀：
base64加密，z0，eval，base64

蚁剑：
@ini_set("display_errors","0")
@set_time_limit
做混淆：execute
做混淆：0x....=

冰蝎：
冰蝎2：第一阶段，返回包状态码为200，响应包返回内容必定是16位的密钥，强特征是 accept 里面有个 q=.2
冰蝎3：请求包中content-length 为5740或5720（根据Java版本）
Accept&Cache-Control
每一个请求头中存在Pragma: no-cache，Cache-Control: no-cache
内置16个ua头
Content-Type: application/octet-stream(新整理)
冰蝎4：默认时，webshell都有一串密钥。该密钥为默认密码rebeyond的32位md5值的前16位
ua头 referer头 accept 默认aes128 秘闻长度16整数倍
请求头解密后
@error_reporting(0);\r
{"status":"success","msg":"

哥斯拉：
cookie结尾存在分号，pass 字段，getclass，getclassLoader关键字；payload使用base64编码等特征。
响应方面，哥斯拉会响应三次
还有一个地方需要注意的就是webshell连接，所以一般会设置长时间连接，connection这里会是keep-alive

# 内存马的查杀
基于网络特征 访问api路由 
首先看是从哪打进来的 ，对服务器做排查 是网站漏洞还是组件漏洞
可以先查看web日志是否有可疑的web访问日志，如果是filter或者listener类型就会有大量url请求路径相同参数不同的，或者页面不存在但是返回200的
查看是否有类似哥斯拉、冰蝎相同的url请求，哥斯拉和冰蝎的内存马注入流量特征与普通webshell的流量特征基本吻合。通过查找返回200的url路径对比web目录下是否真实存在文件，如不存在大概率为内存马。
如在web日志中并未发现异常，可以排查是否为中间件漏洞导致代码执行注入内存马，排查中间件的error.log日志查看是否有可疑的报错
想清理的话 就让服务进程重启一下 这是在内存做清除  
后门清除：对遍历进程，根目录的Poc，看环境变量，ssh后门
```
servlet-api类：filter型、servlet型、listener型
spring类：interceptor型、controller型
Java Instrumentation类：agent型
如果是jsp注入，日志中排查可疑jsp的访问请求。

如果是代码执行漏洞，排查中间件的error.log，查看是否有可疑的报错，判断注入时间和方法

根据业务使用的组件排查是否可能存在java代码执行漏洞以及是否存在过webshell，排查框架漏洞，反序列化漏洞。

如果是servlet或者spring的controller类型，根据上报的webshell的url查找日志（日志可能被关闭，不一定有），根据url最早访问时间确定被注入时间。

如果是filter或者listener类型，可能会有较多的404但是带有参数的请求，或者大量请求不同url但带有相同的参数，或者页面并不存在但返回200
```

查杀
```
新增的或修改的；
没有对应class文件的
xml配置中没注册的
冰蝎等常见工具使用的
filterchain中排第一的filter类
```



# fastjson
fastjson原理：
能够执行反序列化的根源定位在 @type
在类中使用json.parse（）造成任意执行，
流量特征：json autotype
在请求包中查找json格式的字符串，重点在于rmi和一些出网操作
new
1、fastjson提供的反序列化功能允许用户传入json格式数据的时候通过@type的value值指定任意反序列化类名
2、fastjson的反序列化机制会将反序列的类进行实例化对象，并调用该对象的setter和部分getter方法
3、恶意用户可以构造payload是目标应用的代码执行流程进入这部分setter和getter方法，如果这些方法中存在Gadget，就会造成一些安全问题。
4、官方采取黑名单过滤的方法，对反序列化的类名进行校验，checkAutoType不断被绕过

fastjson利用流程：
使用@type的value字段执行反序列化的类，例如JdbcRowSetImpl这个类，接着将这个类中的成员变量datasourcename的value值设为rmi远程加载类，这样fastjson在将传入的类反序列化、实例对象后，会通过成员变量传入的value值，请求rmi服务器，最后rmi返回远程类，fastjson执行这个远程恶意类。导致rce漏洞。
fastjson反序列化的特征：
在请求包中查找json格式的字符串，重点在于rmi和一些出网操作

不出网：基于BasicDataSource类

# log4j
原理：该漏洞主要是由于日志在打印时当遇到`${`后，以:号作为分割，将表达式内容分割成两部分，前面一部分prefix，后面部分作为key，然后通过prefix去找对应的lookup，通过对应的lookup实例调用lookup方法，最后将key作为参数带入执行，引发远程代码执行漏洞
特征：${jndi：rmi  ladp

# shiro
shiro550：
返回包中rememberme=deleteme，可能存在
硬编码导致密钥可以被爆破，然后利用密钥进行解密和反序列化构建payload
shiro721：
这次没有了硬编码，但是rememberme字段存在问题了，导致可以构建恶意rememberme的值
服务端收到登录请求后，会对cookie的rememberMe字段的值进行base64解码，接着进行AES解密，然后反序列化
流量特征：rememberme字段长度异常

# weblogic
war后门文件部署，任意文件上传，T3反序列化，流量特征：fileoutstream
weblogic反序列化基于t3协议
流量特征：服务器开放7001端口  传递xml数到wls-wsat  数据包内容有bash或者dnslog字段。

反序列化：特征是xml格式的数据到wls，数据内容可能包含bash或者dnslog之类的 开放7001端口

弱⼝令利⽤上传war的特征：分析AdminServer.log⽇志查找deploy war字样

利⽤T3协议传输反序列化数据特征：AdminServer.log⽇志中，⼀般利⽤的cc链(commoncollections)，因此⽇志⽂件可能有InvokerTransformer、ChainedTransformer等

利⽤socket连接特征：base_domain.log或AdminServer.log⽇志中，会有socket字样

⽂件操作：base_domain.log中会有FileOutputStream字样

T3 cc链的利用，T3协议加上java反序列化，特adminserver.log中有invoketransformer和chainedtransformer字段，T3CC利用原理好像是weblogic调用rmi服务传递对象时对参数没过滤完全，而中间T3协议对传递的字符串只能增加黑名单

特征:数据内包含t3字段，类似：t3 12.2.1\nAS:255\nHL:19\nMS:10000000\nPU:t3://us-lbreens:7001\n\


# struts2

流量特征：遇到之后先进行url解码在查看总体含义
特征是使用OGNL语法，payload中含有memberaccess字段，相关特征应该在catalina.out日志文件中有体现

攻击
stu2-045：
Struts2默认使用org.apache.struts2.dispatcher.multipart.JakartaMultiPartRequest 类对上传数据进行解析.JakartaMultiPartRequest类在处理Content-Type时如果获得非预期的值的话,将会抛出一个异常,对这个异常的处理会对错误信息进行OGNL表达式解析,从而造成了恶意代码执行（接受content-Type值的时候会使用OGNL表达式解析，造成远程代码执行漏洞）

# redis未授权
原理：redis使用了默认配置，使端口绑定在了0.0.0.0:6379并且暴露在公网的话
此时我们任意一台带有redis-cli的机器就可以直接访问，跳过登陆验证，从而可以写shell，写入shell的话使用config参数

# thinkphp
5.x通杀原理：路由url从Request::path()中获取，由于var_pathinfo的默认配置为s，我们可利用$_GET['s']来传递路由信息，也可利用pathinfo来传递，但测试时windows环境下会将$_SERVER['pathinfo']中的\替换为/。结合前面分析可得初步利用代码如下：index.php?s=index/\namespace\class/method
正则没写好

# 溯源反制
溯源画像技术---ip注册信息，云服务器，vps，社工库，支付宝，src排行榜
溯源反制技术---直接反制攻击机，钓鱼邮件反制，蜜罐，工具反制（反制awvs，sqlmap，cs等）
等）
