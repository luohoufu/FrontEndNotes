[TOC]

#一、选择器

##基本选择器
* ／* 匹配所有的标签
* .class
* /#id
* 标签

##层次选择器

* E F
* E>F 只选择直接子代
* E+F 相邻选择器， 选择在后面的
* E~F 后面的所有元素 E后面的所有标签 兄弟
##动态伪类选择器
* E:link
* E:visited
* E:hover
* E:active

love hate
## 属性选择器

* x[title] 选择有title属性的锚标签
* x[href='foo'] 选择属性值为。。的标签
* x[href*="nettus"] 包含nettus的 **可以使用自定义的属性**
* x[href^='a']以a开头的
* x[href$='.jpg']以.jpg为结尾的

## 伪元素

* x：checked
* x:after
* x:before
* First-line first-letter

## 第几个

* :nth-child(n) 1start.    Nth-last-child(n) 孩子第几个
* Nth-of-type(n)   Nth-last-of-type(n)  元素类型第几个。 X:first-of-type 伪类可以让你选择该类型的第一个兄弟节点


* First-child last-child
* Only-child. 唯一一个的时候，才设置为红色 only-of-type

## 优先级

- 权重记忆口诀。从0开始，一个行内样式+1000，一个id+100，一个属性选择器/class或者**伪类**+10，一个元素名／或者伪元素+1.


- 伪类表示的是一种“状态”比如hover，active等等，而伪元素表示文档的某个确定部分的表现，比如::first-line 伪元素只作用于你前面元素选择器确定的一个元素的第一行。

## 权重规则

- 相同的权重，以后面出现的选择器为最后规则
- 不同权重，权重高的生效

行内。  id。class伪类属性选择器。  元素名伪元素