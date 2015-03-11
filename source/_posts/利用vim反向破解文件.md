title: 利用vim反向破解文件
date: 2015-03-10 23:00:06
categories:
- linux
tags:
- vim
- security
- linux
---

#前言
前几天和[wilbeibi](http://wilbeibi.com)聊到CS615的最后一次大作业，老师开放漏洞让我们去攻击学校的服务器占领首页，然后各小组之间24小时内互相攻击，于是去年做的时候他去服务器上偷偷事先把别人的ssh私钥下载下来了然后取得了胜利，放了一个超级马里奥的游戏上去。。。。。实在是太贱了。。。。

然后[wilbeibi](http://wilbeibi.com)继续吹比说，他的文件目录权限设置的特别好，即使一个group的也几乎啥都看不到，不信让我去看看，然后我就真的去看了看，这是配图。。

![Imgur](http://i.imgur.com/IxFMTUW.png)

乍一看，除了几个作业的目录和无关紧要的配置文件，别的权限基本都不可读，全被设置好了，但是这一组文件引起了我的兴趣

![Imgur](http://i.imgur.com/PTpwXsp.png)

<!--more-->
#VIM

`.bashrc`也就是bash的配置文件，而`.bashrc.swo`是使用**vim**时留下的恢复文件，有时候由于各种原因，比如电脑断电，网络原因导致ssh链接断开等等，在编辑vim的时候异常退出了，vim还是能帮你恢复之前正在编辑但是还没有保存的文档，这种机制就需要vim自带的恢复文件，一般第一个恢复文件是叫`.swp`，第二个是`.swo`，当再次打开原来的文件时，就会提示你恢复文件。

而在这个系统中，`.bashrc`是不可读的，所以我自然不可能从这个文件下手，但是`.bashrc.swo`是可读的，所以我直接把这个文件拷贝了出来，想试试能不能恢复，我试图直接打开这个文件

	vim .bashrc.swo
	
但是结果都是乱码

![Imgur](http://i.imgur.com/siXmHsLm.png)

于是我想试试能不能自己编写一个`同名空文件`，然后让vim自己的机制来恢复回来呢,于是。。。。

```
lliu19@rainman:~/tmp $ touch .bashrc
lliu19@rainman:~/tmp $ mv .bashrc.swo .bashrc.swp
lliu19@rainman:~/tmp $ ll
total 25
drwxr-xr-x+  2 lliu19 student     4 Mar 10 23:28 .
drwxr-xr-x+ 12 lliu19 student    20 Mar 10 23:28 ..
-rw-r--r--+  1 lliu19 student     0 Mar 10 23:28 .bashrc
-rw-r--r--+  1 lliu19 student 12288 Mar 10 23:25 .bashrc.swp
lliu19@rainman:~/tmp $ vim .bashrc

```

![Imgur](http://i.imgur.com/208SiKw.png)

大功告成！！！

于是我成功反向恢复了他的配置文件，知道了他ec2的私钥文件地址和文件名，虽然光知道地址还是不能知道破解，但是通过vim自带的恢复机制破解了别人的文件，还是很爽的。。。

![Imgur](http://i.imgur.com/8pSpjLR.png?1)

#WHY

为什么这一招能奏效了，这和linux的权限设置有关，linux在默认创建文件时使用`umask`去配置权限，例如这是学校服务器的默认umask

```
lliu19@rainman:~/tmp $ umask
0022
```

也就是说创建文件时默认的是去掉了w权限，保留了r+x权限，所以当在服务器上编辑文件时不小心留下了vim的恢复文件`.swp`时，就会有权限问题。

但是vim毕竟是老牌编辑器不会想不到这个问题，所以vim在生成`.swp`恢复文件时，和源文件是一样的读写权限，大家可以去试试。因此之所以会在服务器留下这个文件，有各种原因，有可能在设置权限时没想到有这么贱的方法，只修改了原来的权限，漏掉了vim的配置文件。

虽然这次只是破解了一个配置文件，但是vim编辑的文件里经常会有重要的配置，密码，密钥等等，所以还是要小心的。

 


