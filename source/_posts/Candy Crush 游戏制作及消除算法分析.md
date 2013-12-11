title: Candy Crush 游戏制作及消除算法分析
date: 2013-12-10 22:45:45
categories: cocos2d-x #文章文类
tags: cocos2d-x
---
***本文为firedragonpzy原创,转载务必在明显处注明： 
        转载自【Softeware MyZone】原文链接: http://www.firedragonpzy.com.cn/index.php/archives/4119***

每日一曲【程序猿需要激情，high起来吧！】
  <embed src="http://www.xiami.com/widget/0_3515679/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>
最近做类似Candy Crush的消除类游戏，现在和大家分享下关于candy移动消除这一块，望大神们多多指点……

首先，移动后candy的消除分为两大类，第一大类就是你移动交换的两个candy发生消除，例如你移动百消球和临近的普通candy进行交换，这时候消除和普通candy颜色一样的candy。

第二大类是你移动后，相临的大于等于三个candy的可以产生消除。
<!--more-->
##现在分析下第一大类：两个交换移动之后产生消除。
这一大类有以下两种情况：

* 相同颜色的candy
* 不同颜色的candy

对了，在这里说一下candy的创建。
candy我是利用工厂模式创建的，第一层分类为颜色，第二层分类为某种颜色下横【横闪】、竖【纵闪】和方块【爆炸圈】，最终肯定有个特殊【百消球】。

*关于相同颜色的candy*：
这时候分为以下几种情况：

* 百消球 和 百消球
* 爆炸圈 和 爆炸圈
* 爆炸圈 和 纵横闪
* 纵横闪 和 纵横闪
* 爆炸圈 和 普通
* 纵横闪 和 普通
* 普通 和 普通

其中前四类是需要特殊处理的，后三类列为其他情况即可。其他情况只需要进行个返回原位置操作即可。

*关于不同颜色的candy*：

这时候分为以下几种情况：

* 百消球 和 爆炸圈
* 百消球 和 纵横闪
* 百消球 和 普通
* 爆炸圈 和 爆炸圈
* 爆炸圈 和 纵横闪
* 纵横闪 和 纵横闪
* 爆炸圈 和 普通
* 纵横闪 和 普通
* 普通    和 普通
其中前六类是需要特殊处理的，后三类列为其他情况即可。其他情况只需要进行个返回原位置操作即可。

##然后分析下第二大类：移动后，相临的大于等于三个candy的可以产生消除。
这时候先分析下如果相邻的没有特殊的candy，即交换能构成消除，三个candy消除，四个根据方向形成纵闪或横闪，五个纵或横形成百消球，五个但是带折角的形成爆炸圈。这时候在交换的那个位置形成特殊的candy即可。

在完成这个操作之后，可能会产生特殊的candy，这时候如果我们再进行移动交换，移动完毕后就可能在这相邻的里面有特殊的，我们先处理这些特殊的，然后再和原有该消除的合并即可。

关于数据的存储处理，大家可以在candy中加个标识，表示当前candy是否加入到了vector【即将被消除的candy】中。

消除类游戏主要是写好逻辑算法，让其达到最优。我制作游戏期间出现最多的问题就是这个应该消除的candy怎么个存储法。

##主体思路，简单说下：

在主场景中加入你的游戏层，我习惯性是GameLayer，此文件中两个类，一个是CGameLayer,一个是CChessBox，CChessBox就是你的candy布局，在这里我称之为棋盘，注意CGameLayer创建的时候要留好接口，比如关卡信息，游戏层的位置。为什么要留游戏层的位置这个接口呢？因为你的棋盘不可能是固定不变的，不同的关卡位置可能发生变化，这样子你留好了位置接口之后就可以在不再用引擎那费劲的转化，到时候自己再判断区域的时候处理下x/y即可。

说完接口，我们简单再说下这个棋盘，为什么要有棋盘呢？因为我们看到的cand的移动，其实就是忽悠人的，我们程序操作的都是棋盘。下面看下我的棋盘接口：
```c++
public:
    CChessBox();
    ~CChessBox(){}
    
private:
    CC_SYNTHESIZE(int, m_nRowIndex, RowIndex); // 行索引
    CC_SYNTHESIZE(int, m_nLineIndex, LineIndex); // 列索引
    
    CC_SYNTHESIZE(short, m_nRowUpBoxNums, RowUpBoxNums);// 行上的candy数
    CC_SYNTHESIZE(short, m_nRowDownBoxNums, RowDownBoxNums); // 行下的candy数
    CC_SYNTHESIZE(short, m_nLineLeftBoxNums, LineLeftBoxNums); // 列左的candy数
    CC_SYNTHESIZE(short, m_nLineRightBoxNums, LineRightBoxNums); // 列右的candy数
    
    CC_SYNTHESIZE(CCRect, m_pRect, Rect); // 是否点击candy
    CC_SYNTHESIZE(CCRect, m_pAtlasRect, AtlasRect); // candy和candy的碰撞区域
    CC_SYNTHESIZE(CCPoint, m_pCenterPoint, CenterPoint); // candy的位置
    CC_SYNTHESIZE(CBoxContent*, m_pBoxContent, BoxContent); // 棋盘存放的内容
};
```
其中CBoxContent如下：
```c++
class CBoxContent : public CCObject {
    
public:
    CBoxContent() : m_pSkin(NULL), m_nRowIndex(0), m_nLineIndex(0)
    {
    }
    
private:
    CC_SYNTHESIZE(CCandy*, m_pSkin, Skin); // 皮肤及表示candy
    CC_SYNTHESIZE(int, m_nRowIndex, RowIndex);
    CC_SYNTHESIZE(int, m_nLineIndex, LineIndex);
};
```
在游戏层中存放个可变长的CChessBox数组，这样子游戏的棋盘就出来了。点击移动操作什么的都是针对棋盘，而CBoxContent就是个表现，有些同学这时候会问为什么为什么要有CBoxContent,其实我之前的行上列左等都是CBoxContent的属性，后来调节给了CChessBox，行上等属性在CBoxContent中很不方便，另外在CBoxContent添加m_pSkin存放candy也是为了后期的扩展。

友情提醒下，就是移动后candy的删除问题。你最好在你的工厂中保存下释放的candy，这样子可以提高效率，避免每次都创建精灵。

我的工厂就提供了释放和新建CBoxContent方法，如下:
```c++
void IFactory::freeBoxContent(CBoxContent* boxContent)
{
	boxContent->getSkin()->retain();
	boxContent->getSkin()->setVisible(false);
	boxContent->getSkin()->removeFromParentAndCleanup(true);
	m_pFreeBoxContentVector.push_back(boxContent);
	delete boxContent;
	boxContent = NULL;
}

CBoxContent* IFactory::getNewBoxContent()
{
	CBoxContent *box = NULL;
	if (m_pFreeBoxContentVector.size() > 0)
	{
		box = m_pFreeBoxContentVector.back();
		m_pFreeBoxContentVector.pop_back();
		box->getSkin()->setVisible(true);
	}else
	{
		IFactory *factory;
		short typeFlag = CCRANDOM_0_1()*m_nCandyTypeNums;
        
		switch (typeFlag)
		{
            case CANDY_TYPE_GREEN:
                factory = dynamic_cast<CFactoryGreen*>(new CFactoryGreen());
                break;
            case CANDY_TYPE_ORANGE:
                factory = dynamic_cast<CFactoryOrange*>(new CFactoryOrange());
                break;
            case CANDY_TYPE_PURPLE:
                factory = dynamic_cast<CFactoryPurple*>(new CFactoryPurple());
                break;
            case CANDY_TYPE_RED:
                factory = dynamic_cast<CFactoryRed*>(new CFactoryRed());
                break;
            case CANDY_TYPE_YELLOW:
                factory = dynamic_cast<CFactoryYellow*>(new CFactoryYellow());
                break;
            default:
                factory = dynamic_cast<CFactoryGreen*>(new CFactoryGreen());
                break;
		}
        
		box = new CBoxContent();
		box->setSkin(factory->_doCreateTypeByFactory());
	}
	return box;
}

```
哦，不早了，该洗洗睡了，就先写这些吧，后续补充……

----------
        欢迎热爱编程的朋友们参与到cocos2d-x编程中，为了给大家提供良好的交流环境，网站以开启QQ群

                     Software MyZone：66202765（群号，欢迎加入,若满，请加1群）

                              Software MyZone 1群(2dx)：286504621

                         【加群请写：Software MyZone或者是firedragonpzy】

                         淘宝店：【应小心的易淘屋】初次开店，大家多多支持……

                      群论坛：火龙论坛正试运营阶段，欢迎大家多提些建设性意见……
