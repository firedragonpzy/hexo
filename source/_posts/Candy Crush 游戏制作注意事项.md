title: Candy Crush 游戏制作注意事项
date: 2013-12-18 22:45:45
categories: cocos2d-x #文章文类
tags: cocos2d-x
---
***本文为firedragonpzy原创,转载务必在明显处注明： 
        转载自【Softeware MyZone】原文链接: http://www.firedragonpzy.com.cn/index.php/archives/4159***

每日一曲【咱们结婚吧！】
 <embed src="http://www.xiami.com/widget/27050813_1772301196/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>

写了第一篇[游戏制作及算法分析](http://www.firedragonpzy.com.cn/index.php/archives/4119)之后，本来想写分析二来，后来考虑一下，还是先和大家分享下注意事项吧，或者说是这其中的易错点，在这期间也捎带说下部分算法，这样子基本就全了。

`下面列举下注意事项：`
<!--more-->

* candy的创建及消除
* candy消除后的下落
* candy的展现
* candy全局检测提示

##下面先来讲述下：candy的创建及消除
关于创建和消除，在了第一篇[游戏制作及算法分析](http://www.firedragonpzy.com.cn/index.php/archives/4119)已经提及到，要在工厂中保存部分candy，这样能提高效率，防止每次不必要的创建以及消除。
##接来讲述下：消除后的下落
关于下落，我们首先需要保证的是下落的速度一致，动作需要传入一个时间t，根据 t = s/v，我们把v定义为常量，每次获取距离即可。说完时间的获取，这时候我们就得全盘遍历棋盘，进行candy的下落处理。这里有个需要注意的地方，我们遍历，先遍历列再遍历行，平时的习惯是行了之后再遍历列。这里说下为什么先遍历列，因为你要保证下落的candy是连续的，假设一列有三个空缺，那你就需要在当前列的最后一行再加三行，每遍历完当前列的最后一行，重置下你的临时位置，让其再从最后一行的上一行开始。如果这里使用行列遍历，这时你后来新添加的candy容易发生位置错乱。
##接来讲述下：candy的展现
关于游戏中使用到的candy，我们可以使用tp打包，这样子我们在程序中就可以加载一张纹理，这里也可以进行纹理的预加载，保证游戏的流畅性。多个candy打包后的另外一个好处就是我们可以使用精灵表，减少了渲染批次，大大提高了效率。我们把精灵表加载到游戏层，完后我一个个candy加入到精灵表中，精灵表上的个数为棋盘的个数。这就必须保证每次我们销毁一个candy的时候进行精灵的持有、设置不显示和从精灵表上移除的操作。
##最后再说下candy的全局检测提示
关于检测，我这里提供两种方案：

 1. 模拟移动，即你将candy移动到需要的地方进行周边检测 
 2. 每次获取三个进行周边检测，获取相邻三个，检测下是不是有两个同类型的，如果有则记录下在这三个之中不同类型的candy的位置。完后进行周边检测。假设我们三个的标识分别为0、1、2，则0只需要检测上、左和下边即可，1只需要检测上和下边即可，2只需要检测上、右和下即可。这里要注意边界情况的处理。

关于这个检测，我们要考虑到特殊主体，上面只是讲解了普通主体的判断，注意：普通主体判断我们只需要检测三个即可。

说到特殊主体，这个容易被忽略。超级主体：百消球，只要有一个即可，除了百消球和普通主体，其余的只要有两个相连即可提示。

目前就这些问题，如果有其余注意事项，后续补充……
----------
<div align="center"><span style="color: red;">欢迎热爱编程的朋友们参与到cocos2d-x编程中，为了给大家提供良好的交流环境，网站以开启QQ群
Software MyZone：66202765（群号，欢迎加入,若满，请加1群）
Software MyZone 1群(2dx)：286504621
【加群请写：Software MyZone或者是firedragonpzy】
**<span style="color: #800000;">淘宝店：<span style="color: #0000ff;">[<span style="color: #0000ff;">【应小心的易淘屋】</span>](http://roy-pang.taobao.com)</span>初次开店，大家多多支持……</span>**
群论坛：[<span style="color: blue;">火龙论坛</span>](http://bbs.firedragonpzy.com.cn/forum.php)正试运营阶段，欢迎大家多提些建设性意见……</span></div>