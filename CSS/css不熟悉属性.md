#一、文本属性
##font
* font-style(normal,italic,oblique,inherit)
* font-variant(normal,small-caps小型大写字母，inherit)
* font-weight(normal,bold,bolder,lighter,100-900)
* font-size/line-height
* font-family
* 
##direction
*  ltr
*  rtl

##letterspacing（字符间距）

##lineheight
* line-height：150% 会根据父元素的字体大小先计算出行高，再让子元素继承。
* 1.5em  会根据父元素的字体大小先计算出行高，再让子元素继承 
* 1.5  会根据子元素的字体大小动态计算行高
##text-decoration
* underline
* overline
* line-through
* blink 闪烁

##text-indent
首行缩进 

##text-transform
控制文本大小写

* capitalize 每个单词以大写字母开头
* uppercase 大写
* lowercase 小写
* inherit

##white-space
* pre  空白保留
* nowrap 不换行
* pre-wrap 保留空白 正常换行
* pre-line 合并空白符序列 保留换行符
* inherit

##word-spacing 单词间距

#二、box
##overflow
* visible 不裁剪，可能会显示在内容框外
* hidden 裁剪 不滚动
* scroll 滚动
* auto 溢出 提供滚动机制
* no-display 不适合框则删除框
* no-content 不适合框 隐藏内容

#三、字体属性
##font
* font-style [normal italic oblique inherit]
* font-variant 设置小型大写字母 small-caps
* font-weight [normal bold bolder lighter 100-900] 400=normal 700=bold
* font-size/line-height
* font-family
* caption

##lineheight行间的距离

一般line-height 为20px
；应用到块级元素时，定义了该元素中基线之间的最小距离

* number 与当前的字体尺寸相乘设置行间距  子元素根据自身字体计算
* length 固定行间距 一般为20px
* % 基于当前字体尺寸的百分比行距 子元素直接继承

#四、内容生成
##content
与beforeafter伪元素配合使用，茶如生成内容
#五、列表属性
##list-style(none)
* type [none disc(默认 实心圆) circle（空心圆） square（实心方块） decimal（数字）decimal-leading-zero（0开头的数字标记） lower-roman uper-roman lower-alpha ....]

* positon inside(文本内 环绕文本根据标记对齐) outside（文本左侧 间隔距离）

* image url（）图像


#五、定位属性
##clip
clip规定一个元素的可见尺寸
<pre>
img
  {
  position:absolute;
  clip:rect(0px,60px,200px,0px);
  }
</pre>
##cursor
鼠标形状
##vertical-align
* baseline
* sub文本下标
* super 文本上标
* top 行中最高元素顶端
* text-top 父元素字体顶端
* middle
* bottom 最低元素
* text-bottom
* %使用lineheight的百分比值来排列次元素 可用负值
##visibility
* visible
* hidden
* collapse 删除一行或一列

#六、表格属性

##border-collapse
是否合并表格边框

* seperate
* collapse 合并为一个单一的边框
##border-spacing
* 水平间距  垂直间距
##caption-side
表格标题的位置

* top
* bottom
##empty-cells
空表格是否绘制边框

* hide
* show
##table-layout
表格布局，是否固定

* automatic
* fixed   固定 由表格宽度和列宽度设定

实现表格不溢出
<pre>
<code>
.table-fix {   
    table-layout:fixed;     
} 

.txt-ell {   
    whitewhite-space:nowrap;  //强制在一行显示
   
    overflow:hidden;    //溢出的内容切割隐藏   
    text-overflow:ellipsis; //当内联溢出块容器时，将溢出部分替换为…   

    word-break:keep-all;  //允许在单词内换行         
}  
</code></pre>

##cell-spacing
单元格之间的距离
##cell-padding
单元格内的padding

#七列表

##list-style
	ul
	  {
	  list-style:square inside url('/i/arrow.gif');
	  }

##list-style-type 列表样式前面是点罗马数字之类
##list-style-positon 
* inside 标记放在文本以内
* outside 标记放在文本外 默认
##list-style-image 标记设置图形