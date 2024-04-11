title: Linux 应急响应命令总结（未完工）
author: John Doe
date: 2022-11-17 13:44:58
tags:
---
# CPU 信息

CPU 信息：lscpu

![upload successful](./images/pasted-110.png)

# 操作系统信息

操作系统信息：uname -a

![upload successful](./images/pasted-111.png)

操作系统信息：cat /proc/version

![upload successful](./images/pasted-112.png)

# 模块信息

模块信息：lsmod

![upload successful](./images/pasted-113.png)

# 账户信息

系统所有账户

系统所有账户：cat /etc/passwd

![upload successful](./images/pasted-114.png)

超级权限账户

超级权限账户：awk -F: '{if($3==0)print $1}' /etc/passwd

![upload successful](./images/pasted-115.png)

可登录账户

可登录账户：cat /etc/passwd | grep '/bin/bash'

![upload successful](./images/pasted-116.png)

最近20条登录失败信息

最近20条登录失败信息：lastb | head -n 20

![upload successful](./images/pasted-117.png)

所有账号最后登录信息

所有账号最后登录信息：lastlog

![upload successful](./images/pasted-118.png)

最近20条登录信息

最近20条登录信息：last | head -n 20

![upload successful](./images/pasted-119.png)

当前登录账号信息

当前登录账号信息：who

![upload successful](./images/pasted-120.png)

空口令账号

空口令账号：awk -F: '{if($2==0)print $1}' /etc/shadow

![upload successful](./images/pasted-121.png)

启动项

启动项：ls -lat /etc/init.d/

![upload successful](./images/pasted-122.png)

启动项：cat /etc/init.d/rc.local

![upload successful](./images/pasted-123.png)

启动项：cat /etc/rc.local

![upload successful](./images/pasted-124.png)

计划任务

当前计划任务

当前计划任务：crontab -l

![upload successful](./images/pasted-125.png)

# Linux Crontab 计划任务


