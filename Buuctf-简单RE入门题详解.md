title: '[Buuctf]简单RE入门两道题详解'
author: Twit
date: 2024-03-30 19:12:52
tags:
---
很多事情都有捷径，但Reverse绝对不包含在内。

逆向工程是向下走的一个过程，从交互性好，可读性好到系统的内核，这是一个很艰辛的过程。

题目均来自buuctf。
##### easyre 1
查壳：

![upload successful](/source/images/pasted-52.png)

根据查克软件Exeinfo PE查看后 显示 64位应用，无壳。


直接拖入ida pro x64 静态调试

![upload successful](/source/images/pasted-53.png)

shift+F12打开字符串窗口，寻找关键字符串“flag”


![upload successful](/source/images/pasted-54.png)

拿到flag
##### reverse_1
同样的步骤，第一步查壳：

![upload successful](/source/images/pasted-56.png)

64位应用，无壳。

拖入ida pro x64，打开字符串窗口寻找flag字符串

![upload successful](/source/images/pasted-58.png)
双击跟进

![upload successful](/source/images/pasted-59.png)
ctrl+x查看什么函数调用了它

进入程序框图，

![upload successful](/source/images/pasted-60.png)
发现short loc_140011996函数处产生了分支，F5反编译为伪c代码。

![upload successful](/source/images/pasted-61.png)

通过对伪c代码的分析，初步知道程序逻辑首先需要输入一个flag，然后对flag进行了一个判断，输出两个结果：

一个是"this is the right flag!\n"

一个是"wrong flag\n"

如果c功底好点的话就能看出 __if__ 里进行了什么判断。

下面附上strncmp（）函数的讲解:

![upload successful](/source/images/pasted-62.png)

在这段程序里就是比较了 __str1__ 和 __str2__ 是否相等，如果相等返回0,

0是布尔类型的假，！0就是真了。

简单来说，如果两个字符串相等则执行if语句。不相等就执行else语句，那么很容易得出，这里是对flag的值进行了校验。

这里需要有一个逆向思维，我们需要往"前"想，结合前面我们发现的程序逻辑是要我们输入一个flag，那么这里的校验就是把我们输入的flag和他实际的flag进行校验，

我们向上分析程序，

![upload successful](/source/images/pasted-64.png)

在输入flag提示下面紧跟了一个函数中有后面校验flag函数中的一个参数 __str1__ 。

那么我们可以推测sub_14001128F()函数应该是类似于scnaf()函数。

它存储的是我们的输入值，我们现在需要的是系统的校验值，那么只能是 __str2__ 

再往程序上看，

![upload successful](/source/images/pasted-65.png)
字符串是可以当作一个数组来看待的，结合前面的一堆函数，我们可以大致推断
if判断的含义是:

程序对 __str2__ 数组里的每个元素进行了遍历，在遍历元素的同时，判断 __str2__ 数组里是否有字符的ASCII十进制编码是111，如果有，就改成ASCII为48的字符串，

那么我们就很清楚的得到了，程序是将 __str2__ 数组里的“o”改成了“0”。

其实ida pro 提供了ASCII值的转换，选中对应的ASCII按R可以自动转化为字符串，

*“题外话:RE对c语言的要求还是比较高的，建议认真去学学”*

那我们现在就是要找出 __str2__ 数组里究竟存在哪些元素，我们可以再次双击__str2__来跟进。

![upload successful](/source/images/pasted-66.png)

在汇编语言中字符，字符串，字节数值数组都由db来定义，所以这里相当于定义了 __str2__ 为{hello_world}。

好的，那么我们现在来理一下思路。

程序的逻辑是：

用户输入flag→校验flag→输入正确/错误。

更深入校验flag的逻辑是：

定义了字符串 __str2__ 为{hello_world}，对字符串 __str2__ 进行遍历，将其中全部的“o”换成"0"，得到新的 __str2__ {hell0_w0rd}再与用户输入的flag进行比较。

对于本题中的变换，我们完全可自己手动将 __str2__ 进行变换为正确的flag，但是对于RE后续的题目，我们可能就要编写python脚本来实现对flag的还原了，因此建议从开始就养成编写脚本的习惯，培养自己的编程能力，对于本题的flag还原可以编写如下脚本。

```python
str2 = "hello_world"

str2 = str2.replace('o', '0')

output = f"flag{{{str2}}}"
print(output)

```


![upload successful](/source/images/pasted-68.png)



