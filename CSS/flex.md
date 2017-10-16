[TOC]



flex-direction：column row  -reverse

flex-wrap：wrap no-wrap wrap-reverse

flex-flow

justify-content：flex-start end center space-between around大一倍

align-items：start end center stretch（默认值，未设置高度或者设置为auto，将占满整个容器） baseline    竖直方向的对齐方式

align-content。 多根轴线的对齐方式，如果只有一根轴线，不起作用





order顺序 flex         grow - shrink - basis      align-self 自己的堆砌方式



#一、基础

	.box{
	 display:flex
	}
	.box {
	  display:inline-flex	
	}

webkit内核浏览器必须加上-webkit前缀

	.box {
		display:-webkit-flex;
		display:flex;
	}

子元素的float clear vertical-align失效

#二、容器的属性
##flex-direction（主轴的方向，即项目的排列方向）
- row 水平方向
- column 垂直方向 **使用此项时，注意转换，使用其做垂直居中时，注意设置的是高度！ **
- row-reverse
- column-reverse
##flex-wrap（一条轴排不下，如何换行）
- no-wrap 不换行
- wrap 换行
- wrap-reverse 换行，第一行在下方
##flex-flow（flex-directon和flex-wrap简写）
##justify-content（在主轴上的对其方式）
- flex-start 左对齐
- flex-end 右对齐
- center 居中
- space-between 两端对齐，项目之间的间隔相等
- space-around 项目两侧间隔相等，所以项目之间的间隔比项目与边框的距离大一倍
##align-items（定义项目在交叉轴上的对其方式）
- flex-start
- flex-end
- center
- baseline 项目的第一行文字的基线对齐
- stretch 默认 **如果项目未设置高度或设为auto，将占满整个容器的高度。**


##align-content（多根轴线的对齐方式，只有一根轴线不起作用）
- flex-start
- flex-end
- center
- space-between
- space-around
- stretch 默认 **如果项目未设置高度或设为auto，将占满整个容器的高度。轴线沾满整个交叉轴**


#三、项目的属性
##order（项目的排列顺序，默认为0，可为负值，越小越靠前）

##flex-grow（定义项目的放大比例，默认为0，如果存在剩余空间也不放大）
若所有项目的flex-grow属性都为1，而一个项目的属性为2，则前者占据的剩余空间比其他项多一倍。

**这个属性可以创造自适应布局**

**设置了flex不为0，则无法通过width来设置，通过flex-basis来设置**
##flex-shrink（缩小比例，空间不足，项目缩小）
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。
##flex-basis（）
flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

	.item {
	  flex-basis: <length> | auto; /* default auto */
	}
它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

##flex属性（grow，shrink，basis简写）
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

	.item {
	  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
	}
该属性有两个快捷值：**auto (1 1 auto) **和 **none (0 0 auto)**。
建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值
##align-self（单个项目可以跟其他项目不一样的对齐方式）
- auto
- flex-start
- flex-end
- center
- baseline
- stretch
