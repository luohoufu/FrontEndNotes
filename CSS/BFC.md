#BFC是什么
##Box：css布局的基本单位

##Formating context 
页面的一块渲染区域，决定了子元素如何定位，以及和其他元素的关系和相互作用

##BFC
块级格式化上下文，独立的渲染区域，规定了内部块级盒子如何布局，并且与这个区域外部毫不相干

##BFC布局规则
* 内部的box会在垂直方向放置
* 垂直方向的距离由margin决定。属于同一个bfc的两个相邻的box的margin会重叠 ， **每个元素的左边与包含块的左边界接触，即使存在浮动**
* bfc区域不会与floatbox重叠
* bfc就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素
* 计算bfc高度是，浮动元素也参与计算

#哪些元素会生成BFC
* 根元素
* float不为none
* position为absolute或fixed
* display为inline-block, table-cell, table-caption, flex, inline-flex
* overflow不为visible

#作用及原理
##自适应两栏布局

    body {
        width: 300px;
        position: relative;
    }
    
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
    
    .main {
        height: 200px;
        background: #fcc;
    }

>每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。


>BFC的区域不会与float box重叠。

通过触发main生成bfc实现自适应两栏布局。例如使用overflow：hidden触发main生成BFC，生成两栏布局

##清楚内部浮动
>计算BFC的高度时，浮动元素也参与计算

因此触发浮动元素的父元素的bfc即可

##放置垂直margin重叠
使元素分属不同的bfc