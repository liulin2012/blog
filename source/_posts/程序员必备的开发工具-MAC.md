title: 程序员必备的开发工具---MAC
date: 2015-03-06 01:27:26
categories:
- Mac
tags:
- Mac
---
##前言

用Mac也1年多了，前半年基本用来打游戏看电影了，由于mac基于unix而来，游戏比较少，好在我只玩dota2，所以对我来说，mac的娱乐性很足。

真正让我感觉离不开Mac的，是我这半年来的学习和开发，在[yanguango](http://yanguango.com)和[wilbeibi](http://wilbeibi.com)的帮助下，对mac可以说有了全新的认识，也对linux编程，shell，vim有了更深入的了解。

##为什么选择Mac？

对于一个程序猿来说，**效率**永远是第一位的，这不仅体现在学习的效率，还有工作编码的效率。在时间平等的情况下，如果才能有更多的**产出**，这一直是程序员所要思考进步的，如何节省**时间**来换取更高的产出呢?答案是----[能花钱的，就不要花时间](http://daily.zhihu.com/story/3387025)

也许有的人说，Mac太贵，对一个还在上学的学生来说压力太大，确实，2000刀的一台mac对于学生党来说是不小的开支，但是如果你有能力，我**极力**劝你先入手一台mac，为什么呢？----[先有 Mac 还是先有银元？](http://www.cnblogs.com/chijianqiang/p/mmac.html)

那有了Mac如何提高效率了，可以看看这篇文章----[程序员效率指南](http://mp.weixin.qq.com/s?__biz=MzA3NDM0ODQwMw==&mid=206041450&idx=1&sn=3982c8cc45d7c47f0fbc19fe8371490f&scene=0#rd)

**工欲善其事，必选利其器**

下面是我自己的一些经验分享，也是我最常使用的一些软件。
<!--more-->

##应用&效率

###[Alfred 2](http://www.alfredapp.com/)
**必装**，Mac下的神器之一，可以使用简单的命令完成相当多的事情
* 定位文件
* 打开软件
* 计算器
* 查询api
* google搜索
* 翻译单词
* ......

自从用了它之后再也不用Dock了,打开应用，查东西比以前效率快**几倍**！
最关键的时他支持workflow插件，通过各式各样的[插件](http://www.zhihu.com/question/20656680)可以实现各种功能!

![Imgur](http://i.imgur.com/pnugYMsm.png) 
![Imgur](http://i.imgur.com/C71WDmom.png)


###[Evernote](https://evernote.com/premium)
**必装**，平时找到的技术文章，牛人的随笔都可以记录下来,也可以自己写学习笔记，搜索也十分迅速
唯一的不足是有国际版和大陆版两种账号，而且不共享，我就吃了亏，国内的笔记没能同步过来。

![Imgur](http://i.imgur.com/otbPVUim.png)


###[Google Driver](https://www.google.com/drive)
**必装**，平时分享文件，上传东西还在用qq群，U盘么？那你就太老土了，在这个资料日益珍贵的年代，我无法想象如果我的所有资料都没了，让我重头起家，那会是多么浩大的一个工程。
幸好现在有了许多免费的云存储服务，google driver免费的15G空间足够平时使用了，配合github基本上资料不会丢，最关键的是分享或者共同协作一些文章，会相当方便
同类：Dropbox


###[1Password 5](https://agilebits.com/onepassword)
**必装**，最方便高效的密码管理器，这年头安全是多么重要，大家可不要像大表姐一样被泄露了艳照，嘿嘿。
这个软件有一个主密码，对每个不同的网站生成不同的密码进行管理，而且会自动填写页面的密码可以通过icloud或者Dropbox同步

![Imgur](http://i.imgur.com/ncFptrU.png)


###[Moom](http://manytricks.com/moom/)
**推荐**，窗口移动工具，对于程序来说，同时开几个窗口那是家常便饭，一个一个移动真是太麻烦了，好在这个软件完美的帮我们解决了问题


###[Mou](http://25.io/mou/)/[MacDown](http://macdown.uranusjr.com/)
**推荐**，可视化的md编辑器，其实用哪个都行，但是md没有统一的标准，所以不同编辑器的效果有可能不一样。

![Imgur](http://i.imgur.com/FejLedt.png)


###[Pocket](https://getpocket.com/)
**推荐**，readerlater工具，保存网站文章或者微博文章，等有时间了就阅读一下，而且这个软件对移动端的支持非常好


###[AppCleaner](http://www.freemacsoft.net/appcleaner/)
**推荐**，清理软件的程序


###[iStat Menus](http://bjango.com/mac/istatmenus/)
**推荐**，检测自己的电脑的各个参数，对于打游戏的同学，可以手动调风扇转速很方便


##开发

###[Dash](http://kapeli.com/dash)
**必装**，查文档的工具，非常好用节省时间，配合alfred能快速查找api

![Imgur](http://i.imgur.com/NHFJF6m.png)


###[Homebrew](http://brew.sh/)
**必装**，Mac下的包管理器，装东西卸载东西都很方便，一个命令 brew install就完事了，要卸载就brew uninstall


###zsh
**必装**，虽然Mac原装的bash已经很好用了，但是zsh的功能更加丰富，而且全面兼容bash，利用[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)一键安装很方便，而且提供上百种插件，例如自带的git能alias相当多的git命令，还有神器z或者autojump，能记住cd进入过的目录，下一次进入的时候，只要z然后输出目录关键字就能到大，再也不用输出一串超级长的cd命令了。


###[TotalTerminal](http://totalterminal.binaryage.com/)
**推荐**，一款不错的终端，可以通过快捷键下拉，我的时command+command，还提供标签和查询功能，比原生的terminal强大不少。


###[CodeRunner](https://coderunnerapp.com/)
**推荐**，测试一些小代码专用，比如想试试这个函数对不对啊，这个类怎么用啊都可以放进这个软件里跑，各种语言基本都支持。

![Imgur](http://i.imgur.com/PgT7U7um.png)


###[Hexo](http://hexo.io/)
**推荐**，博客利器，关键我比较喜欢用md写东西，所以非常方便，原本用wordpress，但是感觉太繁重了，hexo比较轻量级，配置也方便。


##Editor---VIM
我个人只用vim，所以就说一些插件的经验，emacs党可以跳过


###[pathogen](https://github.com/tpope/vim-pathogen)
插件管理器，很方便的管理自己的插件
同类：vundle


###[nerdtree](https://github.com/scrooloose/nerdtree)
编辑vim时可以树形查看自己的目录结构，分屏操作


###[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
神级代码补全软件，google员工的作品，配置有点麻烦，但是相当好用


###[ultisnips](https://github.com/SirVer/ultisnips)&&vim-snippets
定义了一些代码块，避免了很多重复劳动


![Imgur](http://i.imgur.com/hqlcWNs.png)


##其他

###[Popcorn-Time](https://popcorntime.io/)
老美也用的在线P2P高清1080P的电影的电视剧


###[uTorrent](http://www.utorrent.com/)
迅雷在美国封杀了，所以用这个，在海盗湾下载东西十分快，尤其是那些**你懂得**种子。


###[Steam](http://store.steampowered.com/)
已经有很多游戏支持Mac平台了，平时在上面玩dota2和csgo。


##后记

这几天联合[yanguango](http://yanguango.com)和[wilbeibi](http://wilbeibi.com)对一个windows用户进行了深刻的宣传，像苍蝇一样盯着他说Mac有多好，多牛逼。。。。。最后这个同志在我们不厌其烦的劝导下，上纽约中央车站拿下了一台mac，哈哈哈哈哈哈。。。。


![Imgur](http://i.imgur.com/nxFvrCa.png)



以上所有的软件均为正版，也请大家尊重程序员的劳动，支持正版！

今天正好下雪停课，本着共享开源的精神，写下了这些东西分享给大家，希望有帮助，如果还有哪些Mac下的神器我没发现，也请大家通过屏幕下方的联系方式告诉我，我也会不断更新，谢谢！

作者:lin
邮箱:liulin.jacob@gmail.com
[新浪微博](http://weibo.com/1939168341)
[github](https://github.com/liulin2012)

