[TOC]

#一、获取节点

##1 document
返回NodeList，**具有实时性**
###getElementById
###getElementsByName
###getElementsByTagName



## 2 elelment和node的区别

* node是dom层次结构中的任何类型的对象的通用名称，Node有很多种类型，

  | 常量                          | 值    | 描述                                       |
  | --------------------------- | ---- | ---------------------------------------- |
  | Node.ELEMENT_NODE           | 1    | 一个元素节点                                   |
  | Node.TEXT_NODE              | 3    | element或attr中的实际的文字                      |
  | Node.COMMENT_NODE           | 8    | 注释节点                                     |
  | Node.DOCUMENT_NODE          | 9    | document节点                               |
  | DOCUMENT_TYPE_NODE          | 10   | 描述文档类型的 [`DocumentType`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentType) 节点。例如 `<!DOCTYPE html>`  就是用于 HTML5 的 |
  | Node.DOCUMENT_FRAGMENT_NODE |      |                                          |
  |                             |      |                                          |

* element继承了node类，element是node多种类型中的一种，同时扩展了id、class、children属性

* **children是Element的属性，childNodes是Node的属性**

* Element的children[0]仍为Element，是Node和Element的实例，Node的childNdoes[0]为Node，只是Node的实例，不是Element的实例

##2 作为节点树的文档

###firstChild lastChild

###parentNode childNodes 返回nodelist对象包含textnode 注释等
###previousSlbling nextSibling
获取已知节点的前一个节点  获取已知节点的下一个节点

##3 作为元素树的文档 
###children 返回nodelist对象 parentNode
###firstElementChild,lastElementChild
###nextElementChild,previousElementChild
###childElementCount 返回值和childre.length相等

#二、节点操作
##1 创建节点
###createElement
document.createElement(‘img’)
###createAttribute
document.createAttribute('class')
###createTextNode
var h=document.createElement("H1")
var t=document.createTextNode("Hello World");
h.appendChild(t);
##2 插入子节点
###appendChild
appendChild(所添加的新节点)
###insertBefore
insertBefore(所添加的新节点，已知子节点)
##3 替换子节点
###replaceChild
replaceChild(要插入的新元素，被替换的元素)
##4 删除子节点
###removeChild
var list=document.getElementById("myList");
list.removeChild(list.childNodes[0]);
##5 复制节点
###cloneNode
cloneNode(true[复制当前节点及其所有字节点],false[仅复制当前节点])

#二、属性操作
##1 获取属性
###getAttribute
元素节点.getAtrribute(元素属性名)
##2 设置属性
###setAttribute
元素节点.setAttribute(属性名，属性值)
##3 删除属性
###removeAttrbute
元素节点.removeAttribute(属性名)
#三、文本操作
##1 insertData(offset，string)
从offset指定的位置插入string
##2 appendData(string)
将string插入到文本结点的末尾处
##3 deleteDate(offset,count)
从offset删除count个字符
##4 replaceData(off,count,string)
从offset将count个字符。用string替代
##5 splitData(offset)
从offset将文本结点分成两个节点
##6 substring(offset,count)
返回由offset起得count个结点

# 修改html

* innerHTML
* innerText  表示起始标签和结束标签之间的文本 

```
<div><b>Hello</b> world</div>的innerText为Hello world，innerHTML为Hello  world 
```

* outerHTML   与前者的区别是替换的是整个目标节点，返回元素完整的HTML代码，包括元素本身 
* outerText     与前者的区别是替换的是整个目标节点，问题返回和innerText一样的内容 

## innerHTML与innerText和textConent的区别

![](http://segmentfault.com/img/bVcUm0)

### textContent

* 则 `textContent` 返回 `null`。如果你要获取整个文档的文本以及CDATA数据，可以使用`document.documentElement.textContent`。
* `textContent` 将所有子节点的 `textContent` 合并后返回，除了注释、ProcessingInstruction节点。如果该节点没有子节点的话，返回一个空字符串。
* 在节点上设置 `textContent` 属性的话， **会删除它的所有子节点**，并替换为一个具有给定值的文本节点。

### 与innerText的区别

Internet Explorer 引入了` element.innerText`，目的是相似的，：

* `textContent` 会获取所有元素的内容，包括style和script元素，然而 IE 专有属性 `innerText` 不会
* `innerText` 会受样式的影响，它不返回隐藏元素的文本，但 `textContent` 返回
* `与 textContent 不同的是`, 在 Internet Explorer (对于小于等于 IE11 的版本) 中对 innerText 进行修改， 不仅会移除当前元素的子节点，而且还会永久性地销毁所有内部文本节点（由此导致无法再将这些被销毁的文本节点插入到当前元素或任何其他元素中）