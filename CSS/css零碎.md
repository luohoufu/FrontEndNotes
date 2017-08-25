[TOC]



##1.实现几个列表之间的竖线的实现

###1.:after实现

###2.li+li

###3.分别设置第一个和最后一个的border-right

##2.单行文字超出显示省略号
	overflow:hidden;
	text-overflow:ellipse;
	white-space:nowrap;

##3.多行文字超出显示省略号
###4.webkit浏览器或移动端的页面
-webkit-line-clamp限制一个块元素现实的文本的行数，

	overflow : hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	-webkit-box-orient: vertical;
###5.兼容处理


1. height是lineheight的3倍
2. 结束的省略号用了半透明的png做了减淡效果，或者设置背景颜色
3. ie7-6不显示content内容，所以要兼容6-7可以在内容中加一个标签


		p {
		    position:relative;
		    line-height:1.4em;
		    /* 3 times the line-height to show 3 lines */
		    height:4.2em;
		    overflow:hidden;
		}
		p::after {
		    content:"...";
		    font-weight:bold;
		    position:absolute;
		    bottom:0;
		    right:0;
		    padding:0 20px 1px 45px;
		    background:url(http://css88.b0.upaiyun.com/css88/2014/09/ellipsis_bg.png) repeat-y;
		}


##6.inline-block 中间有间隙
其实是html中之间的换行符（解决方法li不闭合，font-size设为0，设置负边距（不一定浏览器兼容））
##7. 3rem != 30px  chrome中下限最小12px 会自动设为36px
##8.这样表示没有选中第一个元素
	.top ul li + li {
	    border-left: 1px solid #999;
	    margin-left: -3px;
	.top ul {
		text-align: right;      可直接将其设在右边
	}

##9.hacks for dealing with specificity  重复两遍 .btn.btn{}提高优先级 

##10.第一个子元素的margin-top会顶开父元素与父元素相邻元素的间距：
原因：一个盒子如果没有上补白和上边框，那么这个盒子的上边距会和其内部文档流的第一个子元素的上边距重合，这样就会导致第一个子元素将自己的margin当领导的margin执行
解决方式：给父元素增加border-top和padding-top   子元素碰到有效的border或padding就不会越级了

##11.何时使用margin：
需要在border外侧添加空白时
空白处不需要背景色时
上下相连的两个盒子之间的空白。需要相互抵消时，例如：15px+20px的margin将得到20px空白

##12.何时使用padding：
border内侧添加空白
空白处需要背景色时
上下两个盒子之间的空白希望等于两者之和时，

##13.block，inlne-block
* block元素可以包含block元素和inline元素，当inline元素只能包含inline元素，每个特定的能包含的元素也是特定的，比如p元素只能包含inline元素
### block
### inline
* 不会独占一行，多个相邻的行内元素会排列同一行
* **width height 无效**，长度高度内容撑开
* vertical-align属性无效（height属相无效）
* 水平方向padding-left/right margin-left/right 有效，top和bottom无效
### list-item 
### inline-block
### inline-table
### table
### table-cell
### table-column
### table-column-group

##14.margin合并问题
* 块级元素的垂直相邻外边距会合并，
* 行内元素不占上下外边距，左右外边距不合并
* 浮动元素的外边距也**不会合并**

##15.设置placeholder 的样式
**placeholder放在input中即可**

	::-webkit-input-placeholder { /* WebKit, Blink, Edge */
	    color:    #CACACA;
	}
	:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
	   color: #CACACA;
	   opacity: 1;
	}
	::-moz-placeholder { /* Mozilla Firefox 19+ */
	   color: #CACACA;
	   opacity: 1;
	}
	:-ms-input-placeholder { /* Internet Explorer 10-11 */
	   color: #CACACA;
	}
##16.input：placeholder-shown
未输入文字时的样式
##17.选中
	::selection { background:#DDDDDE; }
	::-moz-selection{ background:#DDDDDE; }
	::-webkit-selection { background:#DDDDDE; }

##18.取出按钮input外边的框

	ouline：none；
## 19.height:100%无效

将body，html即所有父元素的height设置为100%；

## 20.消除滚动条

* overflow-y：hidden

* ```
  .container-scroll::-webkit-scrollbar {display:none}
  ```


