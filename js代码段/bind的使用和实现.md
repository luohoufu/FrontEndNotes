```
var altwrite = document.write;
altwrite('hello');//报错
```

alwrite的this指向global或window对象，而write的调用在document上，因此异常

正确的方法：

```
altwrite.bind(document)('hello')
```

由上面的问题引出最简单的用法，创建一个函数，使这个函数不论怎么调用都有相同的this值。像上面的例子，如果不做特殊处理，会丢失原来的对象。

## 偏函数

> 偏函数就是使一个函数接收一些参数，返回一个已经接收了这些预定义参数的函数来接收剩下的参数。

```
fucntion list() {
  reutrn Array.prototype.slice.call(arguments);
}

var list1 = list(1,2,3)
var leadingThritysevenList = list.bind(undefined, 37);

var list2 = leadingThritysevenList();
var list3 = leadingThritysevenList(1,2,3);
```

## setTimeout

setTimeout绑定的是window或global对象。但是使用类的方法时需要this指向类实例，使用bind创建一个绑定了this的函数作为function参数。

```
function Bloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

Bloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
}

Bloomer.prototype.declare = function() {
  
}
```

## 绑定函数作为构造函数

绑定函数也适用于使用new操作符来构造目标函数的实例，当使用绑定函数作为构造实例时，注意：this会被忽略，传入的参数可用。

## 捷径

bind可以为需要特定this值的函数创造捷径。

类数组对象转换为真正的数组。

```Javascript
Array.prototype.slice.call(arg1, arg2, arg3)；
```

## 实现

简单的实现

```javascript
Function.prototype.bind = function(context) {
  return () => {this.apply(context, arguments)}//这里的this会直接绑定词法作用域的this，不会绑定
}
```

考虑到函数柯里化

```Javascript
Function.prototypt.bind = function(context) {
  var args = Array.prototype.slice.call(arguments, 1),
  return () => {
    var innerArgs = Array.prototype.slice.call(arguments);
    var finalArgs = args.concat(innerArgs);
    return this.apply(context,finalArgs); //记得return
  }
}
```

js的函数还可以作为构造函数，那绑定后的函数的调用时，情况就比较复杂，涉及到原型链的传递。

```javascript
Funtion.prototype.bind = function(context) {
  var args = Array.prototype.slice(arguments, 1);
  F = function() {},
  self = this,
  bound = function() {
     var innerArgs = Array.prototype.slice.call(arguments);
     var finalArgs = args.concat(innerArgs);
     return self.apply(this instanceof F ? this : context, finalArgs);
  };
  
  F.prototype = self.prototype;
  bound.prototype = new F();
  return bound;
}
```

通过设置一个**中转构造函数F**，使**绑定后的函数**与调用**bind的函数**处于同一原型链上，用new操作符调用绑定后的函数，不直接绑定的原因，prototype是一个对象，需要new function才能得到，new一个原function占空间太大，因此需要new一个空函数，利用空函数传递。