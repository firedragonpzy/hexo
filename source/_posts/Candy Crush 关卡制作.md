title: Candy Crush 关卡制作
date: 2013-12-30 22:45:45
categories: cocos2d-x #文章文类
tags: cocos2d-x
---
***本文为firedragonpzy原创,转载务必在明显处注明： 
        转载自【Softeware MyZone】原文链接: http://www.firedragonpzy.com.cn/index.php/archives/4174***

每日一曲【程序猿,清醒清醒吧！】
<embed src="http://www.xiami.com/widget/0_93357/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>
今年最流行的游戏莫过于Candy Crush 和 Clash of Clans，英美排行榜前两名被他们包办，不管是iphone还是ipad。那大家知道它们的选关界面是如何制作的吗？

最近制作了一款类似游戏，原版是比着Candy Crush制作的，现在和大家分享下关卡的制作。
<!--more-->
因为之前做惯了《愤怒的小鸟》这种选关类的游戏，思路立马就是分大关小关，关卡状态分为未开启，开启，过关和购买等等一系列的思维变油然而生。但是在实际的实践中，结合Cocostudio来使用的话，这样子并不是最好的。最好的是……

呵呵，先说下游戏的数据吧！关于关卡数据这方面，我们需要两个文件，一个是配置文件，一个是数据文件。我想如果考虑关卡类游戏的话，这样个文件足够了，这样的分配也是比较合理的，同时也有利于后期数据的更新，远远好于一个文件搞定全部。

配置文件即关卡的一些配置，例如关卡中Candy的布局，Candy的类型，移动步数，最终目标等等。

关于目标和限定条件 ，推荐大家使用字典实现，直接用2dx提供的类即可，已经有写入和读取的功能，大家就不必新开辟道路了。【因为可能限定条件下目标套目标等等，大家自己写xml，节点套节点很费劲，还得自己解析】

数据文件即游戏过程中产生的数据，即你当前的得分、星星数、关卡状态等等，这其中有个值得注意的地方，那就是关卡的索引。也就是之前我说的结合Cocostudio来使用的话，分大小关卡可能不是最好的。

在数据文件保存的时候，我们在保存n条数据，也不分关卡了，从0-n即可。这样子在UI和数据提供这方面就好对接了。

数据提供一个数组给UI，UI遍历下就好，这时候有朋友可能有疑问，大关卡的状态如何判定，这里我们可以用每一大关的 最后一关来判定，例如第一大关10小关，判断10有没有开启，如果10开启了，下一大关开启。这样子带来一个问题就是多了几个逻辑判断，不如用大关小关来的快。还有一个问题就是如果每一大关卡的数量不定的话，就不能很智能的实现大关卡的处理。但是好处就是使用Cocostudio后容易数据关联。

```两者各有利弊：```

 - 大关小关卡：少了逻辑判断，容易考虑，但是使用Cocostudio后数据不好关联
 - 小关卡：数据好关联，不能很好的处理各大关卡中小关卡数量不一致的情况。

反正各有利弊，大家自行选择，期待你们对关卡实现的看法……

----------
<div align="center"><span style="color: red;">欢迎热爱编程的朋友们参与到cocos2d-x编程中，为了给大家提供良好的交流环境，网站以开启QQ群
Software MyZone：66202765（群号，欢迎加入,若满，请加1群）
Software MyZone 1群(2dx)：286504621
【加群请写：Software MyZone或者是firedragonpzy】
**<span style="color: #800000;">淘宝店：<span style="color: #0000ff;">[<span style="color: #0000ff;">【应小心的易淘屋】</span>](http://roy-pang.taobao.com)</span>初次开店，大家多多支持……</span>**
群论坛：[<span style="color: blue;">火龙论坛</span>](http://bbs.firedragonpzy.com.cn/forum.php)正试运营阶段，欢迎大家多提些建设性意见……</span></div>