[TOC]

#一、css3边框

##border-radius（ie9+）
% / length
##box-shadow（ie9+）
* h-shadow 水平阴影
* v-shadow 垂直阴影
* blur 模糊距离 编剧模糊
* spread 阴影尺寸 阴影的大小！
* color
* inset 内部阴影

##border-image（ie11+不可，opera）
* border-image-source
* border-image-slice 图片剪裁位置，裁剪，切成九宫格 详解看这里   
* border-image-width border-
* border-image-outset 原有的图像上向外延展
* border-image-repeat (repeat[有可能截断],round[调整尺寸，正好铺满],stretch[拉伸]，space[调整图片间的间距])

#二、css3背景
##background-size（ie9+）
css3前，背景尺寸由图片的实际尺寸决定，现在可以规定背景图片的尺寸，即可以使其拉伸

* length
* %
* cover(背景图片更大)
* contain（div更大）

##background-origin规定背景图片的定位区域

* border-box
* padding-box（默认）
* content-box

##background-clip规定背景的绘制区域

* border-box（默认）
* padding-box
* content-box

##允许添加多张图片
background-image:url(bg_flower.gif),url(bg_flower_2.gif);

#三、css3文本效果
##text-overflow文本溢出
需要与overflow同时使用!

* clip 修建文本
* ellipse 省略符号代表修建的文本
* string 给定字符串代表被修建文本

##text-shadow（ie9+）文字阴影

* h-shadow
* v-shadow
* blur
* color

##word-break字符换行

* normal
* break-all 允许在单词内换行
* keep-all 半角空格或连字符换行

##word-wrap
* normal
* break-word 长单词或url地址内部进行换行

#四、css3字体
<pre> 
@font-face
{
font-family: myFirstFont;
src: url('Sansation_Light.ttf'),
     url('Sansation_Light.eot');
}

div
{
font-family:myFirstFont;
}
</pre>

#五、css3 2D转换
#六、css3 3D转换
#七、过渡transition
* transition-property css属性名称
* transition-duration 过渡效果时间
* transition-timing-function 速度曲线 （linear ease ease-in ease-out ease-in-out cubic-bezier(n,n,n,n)）
* transition-delay 何时开始

#八、动画创建动画 @keyframes
```
@keyframes myfirst
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-moz-keyframes myfirst /* Firefox */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-o-keyframes myfirst /* Opera */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

div
{
animation: myfirst 5s;
-moz-animation: myfirst 5s;     /* Firefox */
-webkit-animation: myfirst 5s;  /* Safari 和 Chrome */
-o-animation: myfirst 5s;       /* Opera */
}
```

animation

* name
* duration
* timing-function
* delay
* iteration-count 被播放的次数（n / infinite）
* direction normal/ alternative反向播放
* play-state paused running 可在js中暂停动画

#九、多列column
* column-count 元素被分割的列数
* column-gap 列之间的间隔
* column-rule width（列之间的宽度） style（solid...） color（） 
* column-span 横跨列数（n/all）
* collumn-width 列宽

#十、用户界面
##box-sizing
* content-box
* border-box

##outline-offset（ie opera不支持）
边框边缘外有一个轮廓 不占用空间

##resize（对元素尺寸调整 大多不支持）
