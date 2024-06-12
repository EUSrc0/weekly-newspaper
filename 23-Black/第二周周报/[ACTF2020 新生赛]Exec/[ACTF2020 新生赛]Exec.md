# [ACTF2020 新生赛]Execkkkv

点击题目链接 http://5e4cbe58-7998-4f70-8535-c2d9e1c3c28b.node5.buuoj.cn:81

![](./d067f1a6b6b200bf1d546c6145c016f.png)

尝试输入127.0.0.1

![](./a0e2064e7554b06f7d09dc77fc1367c.png)

利用 ../ 向上遍历，当传入127.0.0.1;cd ../../../;ls 时看到了flag

![](./f2850ad5d597df6af191263048ab75b.png)

输入127.0.0.1;cd ../../../;cat flag命令获得flag

![](./82d0fb03e8504c131274bde57f0c013.png)

提交flag