

[TOC]

## font-size

几款字体：设置font-size为100px，得到的span height不同

![](https://pic3.zhimg.com/v2-c563ed710d244632fc7a734f52f33e7e_b.png)

字体所占的高度为**content-area**

capital height 680px X-height 485px  ascender 1100px descender 540px(不同平台不同)。em-square是font-size的值

![](https://pic4.zhimg.com/v2-cb05dd4dc6908e522c0f69392ed1fe87_b.png)



## line-height

line-height（不是content-area）用于计算line-box的高度

line-height和content-area的一半加到content-area的顶部，一半加到底部

line-height可以等于、大于、或小于content-area，如果line-height小于content-area，那么leading就是负的，line-box看起来会不内容矮。

line-height设置为1，相对于字体的比例，可能会导致line-height < content-area

**我电脑上的 1117 款字体中，大概有 1059 款字体的 line-height 比 1 大，最低的是 0.618，最高的是 3.378。你没看错，是 3.378！**

![](https://pic4.zhimg.com/v2-efe30b60d553cb6237f9c0cbdebb240b_b.png)

Line-box:

* 内联元素 padding border会增大background 不会增大content-area
* 可替换内联元素（img input svg）、inline-block元素、blockfied、padding、margin和border会增大height，会影响content-area，因为content-area等于line-height 等于height



## vertical-align

默认值是base-line，很少有字体的ascender和descender的比例是一比一的，



[深入理解 CSS：字体度量、line-height 和 vertical-align](https://zhuanlan.zhihu.com/p/25808995?group_id=825729897170345984)