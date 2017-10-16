[js中的执行上下文和调用栈](https://mp.weixin.qq.com/s?__biz=MzA4NjE3MDg4OQ==&mid=2650964999&idx=1&sn=e3287cf874725b0cdcad49d21c5e26f9&chksm=843ae861b34d6177c0b01b214c258bf8d00aeeb6f4e4388be9b54eadc0b3b331e0c6d2165989&mpshare=1&scene=1&srcid=0825NbDXKiZXaWzIcPzLkCvO&key=98a117c12818f031ad38e02754dedb6e267c48959f6aee0516f54c9b961610001048b8f3b8cdbc9a89c3264dc1333a37e014813c1eeb914ac2359bde7eaa71a6eabc2fe285d07ed60d92fea2d85a501d&ascene=0&uin=MTc2NDU3NjM2MA%3D%3D&devicetype=iMac+MacBookPro11%2C2+OSX+OSX+10.12.4+build(16E195)&version=12020110&nettype=WIFI&fontScale=100&pass_ticket=Uh0YYUelD8YXChjg2aL%2F4D%2FrEDjqnzy0HqKFk32XQavPPFaPmwOe8CtlPR5XYjZn)

[一道常被人轻视的前端JS面试题](http://web.jobbole.com/85122/)



[TOC]



关于js的一些面试题总是会有很多坑儿，阅读一下调用栈，最后再结合一下js的经典面试题，看能不能更了解。

## 执行上下文

### what？

js执行时，所在的执行环境很重要

* global 默认
* function 函数体内
* eval 函数内部执行的文本

## 执行上下文栈

进栈出栈的过程

![](https://mmbiz.qpic.cn/mmbiz_gif/d8tibSEfhMMpXcXsVmVvTzffNRmgFWFicWaN0BCh4IPKhyA9KvJd2OxxsZFsFqGLzGYcicE5mnesbB14kfx6h3YRg/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



### 没那么简单，关于每个执行上下文的细节

js解释器内部，对于每个执行上下文的调用会经历两个阶段

* 创建阶段（函数被调用，内部的代码还没开始执行）
  * 创建作用域链
  * 创建变量、函数和参数
  * 决定this的值
* 代码执行阶段		赋值寻找函数引用以及解释、执行代码

### detail

1. 寻找调用函数的代码
2. 在执行函数之前创建上下文
   1. 初始化作用域链 （变量对象 + 所有父级执行上下文中的变量对象）
   2. 创建变量对象
   3. 创建参数对象 检查参数的上下文，初始化其名称和值并创建一个**引用拷贝**
   4. 扫描上下文中的函数声明：对于这些函数，在变量对象中创建与函数同名的属性 ，函数名已存在，**引用值被覆盖**；**函数声明整体提升**
   5. 扫描上下文的变量声明：对于每个发现的变量声明，在变量对象中创建一个同名属性，并初始化为undefined；已存在什么都不做
   6. **确定上下文的this**
3. 代码执行阶段

在上下文中解释执行函数代码，并在代码执行中给变量赋值



## 掉坑儿题

```javascript
function Foo() {
  getName = function() {alert(1)};
  return this;
}

Foo.getName = function () {alert(2)};
Foo.prototype.getName = function () { alert(3)};
var getName = function () {alert(4)};
function getName() {alert(5)};

Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

Foo.getName(); 2

getName() 5 函数声明变量整体提升，getName