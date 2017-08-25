##pre标签
* 预格式化文本
* 保留空格和换行符文本呈现等宽字体
* p address不能包含在所定义的块中 （会导致段落断开）
* 允许链接 图像 水平分割线
* 使用符号实体 &lt代表<

##sub下标 sup上标标签
<pre>
<html>
<body>

<p>
This text contains <sub>subscript</sub>
</p>

<p>
This text contains <sup>superscript</sup>
</p>

</body>
</html>
</pre>

##块元素
默认填充父元素
* div
* p
* ol ul dl

* h123456
* hr
* pre

* table
* form

* address
* blockquote
* dir

##内联元素
* a
* img

* span
* em
* i

* input
* select
* textarea

* br
* cite
* code



##有些行内元素可以设置宽高
可置换元素

* **img**
* **input**
* **textarea**
* **select**


##行内元素和块元素的区别
行内元素设置width无效 height无效 （line-height可以） margin上下无效 padding上下无效

##ol ul dl
* ol有序列表
* ul无序列表
* dl定义列表 dt（定义列表项目） dd（定义列表描述）

<code>
<dl>
   <dt>计算机</dt>
   <dd>用来计算的仪器 ... ...</dd>
   <dt>显示器</dt>
   <dd>以视觉方式显示信息的装置 ... ...</dd>
</dl>
</code>

##doctype的作用
告知浏览器使用哪种html和xhtml规范，分为标准模式，怪异模式，基于框架的html模式，假如浏览器不以doctype标准模式编写DTD，页面除了无法通过代码检验之外，还无法再浏览器中正确显示。

##标准模式和怪异模式的区别
* 使用quiks mode
##noscript 定义浏览器不支持脚本时，显示的内容

