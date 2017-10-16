

![](http://segmentfault.com/img/bVcUmV)



dom是文档对象模型，bom是浏览器窗口和框架，浏览器提供出来访问的其实是BOM对象，从BOM对象再访问dom对象，从而操作浏览器及浏览器读取到的文档； DOM描述的是处理网页内容的方法和接口。BOM描述了与浏览器进行交互的方法与接口。

bom包括history，location，navigator，screen frames

dom包括images[] forms[] links[]

# 什么是BOM

browser  object，主要处理浏览器窗口和框架，浏览器特有的js扩展被认作是BOM的一部分



#什么DOM

document object model 文档对象模型，是为html和xml定义的一个编程接口，

* D可以理解为整个web加载的网页文档
* O对象为类似window对象的东西，可以调用属性和方法，这里指的是document对象
* M模型可以理解为网页文档的树形结构


- 提供了**文档的结构化表示**（节点树状结构）DOM树由节点构成
- 规定了使用**脚本编程语言**应该如何**访问以及操作DOM**
- 下面是一个html dom
  ![](http://segmentfault.com/img/bVcUmB)

#什么阻塞DOM
阻塞渲染的资源（html，css，js）