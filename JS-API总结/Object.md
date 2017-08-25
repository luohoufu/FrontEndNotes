[TOC]



#一、属性

##length
##prototype
#二、方法
##1.创建对象
###assign（把任意多个源对象自身的可枚举属性拷贝给目标对象，返回目标对象）
>Object.assign(target,...sources)
>target:目标对象
>sources：任意多个源对象

```
var obj = {a:1};
var copy = Object.assign({},obj)    //{a:1}
```

* Object.assign()拷贝的属性值
* 目标对象中的属性具有相同的键，属性会被源中的属性覆盖。
* 只会拷贝 **自身的可枚举 ** 的属性到目标对象   ！浅拷贝！
* 在属性拷贝过程中可能会产生异常，比如目标对象的某个只读属性和源对象的某个属性同名，这时该方法会抛出一个 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 异常，拷贝过程中断，已经拷贝成功的属性不会受到影响，还未拷贝的属性将不会再被拷贝。

###create
##2.设置对象
###freeze
###is
###isExtensible
###isFrozen
###isSealed
###preventExtensions
##3.对象原型
###getPrototype
###setPrototypeOf
##4.设置对象属性
###defineProperty
###defineProperties
###
##5.查询对象中的属性
###getOwnPropertyDescriptor
###getOwnPropertyNames
###getOwnPropertySymbols()
###keys
###values
#三、对象原型
- js的所有对象都有object衍生；所有对象都继承了Object.prototype的方法和属性，尽管他们可能被覆盖。
- 原型对象的更改会传播给所有的对象，除非这些属性和方法再原型链中被再次覆盖
##1.属性
###Object.prototype.constructor
###Object.prototype.__proto__
###Object.prototype.__noSuchMethod__

##2.方法
###Object.prototype.__defineGetter__()
###Object.prototype.__defineSetter__()
###Object.prototype.__lookupGetter__()
###Object.prototype.__lookupSetter__()
###Object.prototype.hasOwnProperty()
###Object.prototype.isPrototypeOf()

* 判断对象A是否是B的原型，也就是看看A是否在B的原型链上
* instanceof   判断A是否是B的实例，也就是判断A的原型链上是不是有B

###Object.prototype.propertyIsEnumerable()
###Object.prototype.toSource()
###Object.prototype.toLocaleString()
###Object.prototype.toString()
###Object.prototype.unwatch()
###Object.prototype.valueOf()
###Object.prototype.watch() 