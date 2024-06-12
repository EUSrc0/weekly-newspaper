# [极客大挑战 2019]Secret File

点击题目链接查看题目详情

![](./5c1c1c61436f7303aa147de76fc4d21.png)

F12查看源代码

![](./08819d5df5701eee0ad95f2dc29deb7.png)

发现链接Archive_room.php，访问链接

![](./333e026cd9c63cd44101cb56138020a.png)

从这个页面开始打开BP抓包页面

这里再次点击SECRET

![](./e7f1870b116901ded2927f3ced0ba16.png)

![](./45ed95c67d08afe2fcdd9859fc12c2f.png)

通过抓包，找到了secr3t.php

直接对其进行访问

![](./e5ee9d98a727d640a656cb502dc5280.png)

发现这个没有过滤file

直接使用php伪协议

secr3t.php?file=php://filter/convert.base64-encode/resource=flag.php

![](./d38a455a85d35a55274b7c272080ffb.png)

直接解码获得flag

![](./8a780fe5e4c6e6a8b81a7a850ad236f.png)