[TOC]



#方法

##apply
##call
##bind (绑定函数调用)
> fun.bind(thisArg，args1，args2)

返回由指定的this值和初始化参数创造的原函数拷贝

bind会创建一个新函数（绑定函数），新函数与被掉用函数具有相同的函数体，当被掉函数被调用时this值绑定到bind（）的第一个参数，也就是说this永远不能修改了！**该参数不能被重写**，绑定函数 调用时，bind（）

### 应用

* 绑定函数，是这个函数无论怎么调用都具有相同的this值，直接使用元对象中的方法，一般会丢失原来的对象，使用bind可以解决

  ```
  this.num = 9; 
  var mymodule = {
    num: 81,
    getNum: function() { return this.num; }
  };

  module.getNum(); // 81

  var getNum = module.getNum;
  getNum(); // 9, 因为在这个例子中，"this"指向全局对象

  // 创建一个'this'绑定到module的函数
  var boundGetNum = getNum.bind(module);
  boundGetNum(); // 81
  ```

- 设定函数的预定义参数，然后调用的时候传入其他参数即可

  ```
  function list() {
    return Array.prototype.slice.call(arguments);
  }

  var list1 = list(1, 2, 3); // [1, 2, 3]

  // 预定义参数37
  var leadingThirtysevenList = list.bind(undefined, 37);

  var list2 = leadingThirtysevenList(); // [37]
  var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
  ```

- setTimeout执行 **回调函数中的this** 会自动绑定window或global对象，

  ```
  var num1 = 10;
  var num2 = 10;
  function Num() {
    this.num1 = 1;
    this.num2 = 1;
  }
  Num.prototype.compute = function() {
    setTimeout(this.add,1000);//setTimeout(this.add.bind(this),1000)
  }
  Num.prototype.add = function() {
    console.log(this.num1 + this.num2);
  }
  var a = new Num();//
  a.compute(); //不加bind的结果就是20，因为setTimeout  this指向window或global对象，使用bind将this绑定到实例，并且返回新函数
  ```

- polyfill

  ```
  if(!function.prototype.bind) {
  Function.prototype.bind = function(oThis){
    if (typeof this !== 'function') {
      throw new TypeError ('function ')
    }
    var aArags = Array.prototype.slice.call(arguments, 1),
        fToBind  = this,
        fNOP = function (){},
        fBound = function(){
          return fToBind.apply(this instanceof fNOP ? this:oThis||this, 
        aArags.concat(Array.prototype.slice.call(arguments)))
       }
    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
  }
  }
  ```


## bind的实现

### 概述

创建新函数，新函数与被调函数（绑定函数的目标函数）具有相同的函数体，当目标函数被调用时this值绑定到bind（）的第一个参数，该参数不能被重写。绑定函数被调用时，bind也接受预设的参数提供给原函数。

### 关键思路

* bind不会立即执行函数，需要返回一个待执行的函数（用到闭包，return function（）{}）
* 作用域绑定  使用apply call
* 参数传递，用apply

### 绑定作用与，绑定传参

```
Function.prototype.testBind = function(){
  var _this = this;
  var 
  /*
  *arguments转化为数组，除了第一个参数
  *后面的参数都是输足
  *
  */
  var context = arguments[0]
  args = Array.prototype.slice.apply(arguments,[1]);
  //返回函数,返回的是函数，注意，而且这个函数中进行调用apply执行
  return function(){
    //
    return _this.apply(context,args)
  }
}
```

### 调用不支持动态参数

```
return function(){
  return _this.apply(that,args.concat(Array.prototype.slice.apply(arguments)))
}
```

##toSource 

##toString