##1. 标准css盒子模型，低版本IE盒子模型。box-sizing属性
css3中引入box-sizing属性，默认值为content-box(标准盒子模型)，border-box使用ie盒子模型，即width包括边框的宽度和内边距
##2.css选择器
##4.优先级算法权重
##7.css3新特性
##8.css3的弹性布局
##11.li与li之间有看不见的空白间隔是什么原因引起的，解决办法 display:inline-block 什么时候会显示间隙？
移除空格、使用margin负值、使用font-size:0、letter-spacing、word-spacing

##float及其工作原理
浮动的框可以左右移动，直至它的外边缘遇到包含框或者另一个浮动框的边缘。浮动框不属于文档中的普通流，当一个元素浮动之后，不会影响到块级框的布局而只会影响内联框（通常是文本）的排列，文档中的普通流就会表现得和浮动框不存在一样，当浮动框高度超出包含框的时候，也就会出现包含框不会自动伸高来闭合浮动元素（“高度塌陷”现象）。顾名思义，就是漂浮于普通流之上，像浮云一样，但是只能左右浮动；**脱离文档流**

##BFC（block formating context）及其如何工作
>一些元素，如float。positon为absolute，inline-block，table-cell或table-caption的元素，以及overflow属性不为visible的元素，他们都会建立一个新的**块级格式化上下文**

bfc是html中的一个盒子，只有满足下列条件才能形成

* float属性不为none.

* position属性不为static和relative.

* display属性为下列之一:table-cell,table-caption,inline-block,flex,or inline-flex.

* overflow属性不为visible.

1. 利用bfc可以消除margin-collapse
##列举不同的清楚浮动的技巧，并指出他们各自适用的场景
##15 点击过后的超链接样式不再具有hover和active
link visited hover active  love&hate
##16 link和import区别
@import url("http://www.taobao.com/home/css/global/v2.0.css?t=20070518.css");要放到标签style中

* link是xhtml标签，@import是css提供的一种方式
* link在页面被加载时加载，@import会等到页面全部下载完后再加载，所以import加载css页面开始会没有样式
* 兼容性 ie5以下不支持import
* js可控link，但不可控import

##position属性
* static 默认值 无特殊定位，对象遵循正常文档流top，right,bottom,left等属性不会被应用
* relative 对象遵循正常文档流，将依据属性在**正常文档流**中偏移位置
* absolute 脱离正常文档流 进行绝对定位，**相对于 static 定位以外的第一个父元素进行定位**
* fixed 脱离正常文档流，以**窗口**为参考点定位

##为什么初始化css
##reset和normalizecss之间的区别
normalize
* 保存有用的浏览器默认的样式，不是完全擦除（reset）
* 纠正样式bug（）和各浏览器之间的非一致性
* 用评论和文档来注释样式代码

- normalize收集了有用的浏览器默认样式，reset全部格式化
- normalize纠正了常用样式bug
##css创建三角形原理
将上左右三条边隐藏掉
borderwidth:
border-style:solid
border-color: transparent,transparent,black,transparent;

##display有哪些值？说明他们的作用
##block inline-block inline
###block 
* block元素独占一行，多个block元素会各自新起一行。
* width，height属性可设
* margin，padding属性可设

###inline
* 同行，
* width，height属性无，宽度根据内容变化
* padding margin**水平方线有效** **垂直方向无效**
###inline-block
* width，height可设
##absolute的containing block计算方式跟正常流有什么不同
##经常遇到的浏览器兼容问题，解决方法，常用hack技巧
##z-index和叠加上下文是如何形成的
##可以继承的css属性
##居中浮动元素，居中绝对定位的元素

##css精灵图，如何使用
##CSS里的visibility属性有个collapse属性值是干嘛用的？在不同浏览器下以后什么区别？

##position跟display、margin collapse、overflow、float这些特性相互叠加后会怎么样？

##CSS优化、提高性能的方法有哪些？

##浏览器是怎样解析CSS选择器的？
##在网页中的应该使用奇数还是偶数的字体？为什么呢？

##抽离样式模块怎么写，说出思路，有无实践经验？[阿里航旅的面试题]

##元素竖向的百分比设定是相对于容器的高度吗？

##全屏滚动的原理是什么？用到了CSS的那些属性？

##什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

##视差滚动效果，如何给每页做不同的动画？（回到顶部，向下滑动要再次出现，和只出现一次分别怎么做？）


##如何修改chrome记住密码后自动填充表单的黄色背景 ？

##你对line-height是如何理解的？
##设置元素浮动后，该元素的display值是多少？（自动变成display:block）

##怎么让Chrome支持小于12px 的文字？
##让页面里的字体变清晰，变细用CSS怎么做？（-webkit-font-smoothing: antialiased;）

##font-style属性可以让它赋值为“oblique” oblique是什么意思？

##position:fixed;在android下无效怎么处理？

##为什么响应式设计和自适应设计不同
###自适应布局
为不同的屏幕分辨率定义**布局**，元素不变化但布局变化。
###流失布局
**页面元素的宽度按照屏幕进行适配调整**
###响应式布局
**不同分辨率定义布局**，并且布局中应用流式布局的理念

##隐藏内容的方法
* dispaly：none，visible是对象在网页上不可见，所占空间不改变
* text-indent：-9999px，可以隐藏超链接文本内容，可以用来隐藏图片上方文字，想使用图片作为背景，图片上方放链接文字内容，但html锚文本仍可使用鼠标点击使用，图片作为背景，文字隐藏图片显示出，鼠标也能点击指向。
* overflow：hidden 隐藏溢出内容
* css隐藏滚动条，使用overflow-y：hidden和overflow-x：hidden来隐藏或显示对应横或者竖方向的滚动条

##z-index叠加上下文

z-index就是控制元素在页面的中的叠加顺序，z-index值高的元素显示在z-index值低的前面**。z-index的使用条件：只对有 position 属性的且值不为static的元素才有效**。叠加上下文和“**堆栈上下文**”有关，一组具有共同**双亲**的元素，按照堆栈顺序一起向前或向后移动构成了所谓的堆栈上下文。

## css引入的方式有

* link元素连接到外部的样式文件
* head中style元素
* css中@import标记来导入样式表单
* body中使用style属性来定义样式

## 行内元素有哪些？块级元素

* div p h1 form ul ol
* a b br i span input select

## link和@import

* link是xhtml的标签，@import完全是css提供的一种方式，link还可以具有其他属性
* 加载顺序，页面加载时，link会被同时加载，@import引用的css会等到页面全部被下载完再被加载
* 兼容性的差别，@import是css2.1，老的浏览器不支持
* dom可以控制link标签



