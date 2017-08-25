# 一、简单原型链

父类实例充当子类原型对象

缺点：父类的属性发生变化，子类的属性跟着变化

# 二、借用构造函数

```
function Super(val){
    this.val = val;
    this.arr = [1];
 
    this.fun = function(){
        // ...
    }
}
function Sub(val){
    Super.call(this, val);   // 核心
    // ...
}
```

在子类构造函数中加入执行构造函数！

缺点：

* 每个子类实例都有一个新的fun函数，影响性能（无法实现函数复用）

# 三、组合继承

构造函数加原型

```
function Super(){
    // 只在此处声明基本属性和引用属性
    this.val = 1;
    this.arr = [1];
}
//  在此处声明函数
Super.prototype.fun1 = function(){};
Super.prototype.fun2 = function(){};
//Super.prototype.fun3...
function Sub(){
    Super.call(this);   // 核心
    // ...
}
Sub.prototype = new Super();    // 核心
```

缺点：

* 子类原型上有一份多余的父类实例属性，因为父类构造函数被调用了两次，生成了两份，而子类实例上的那一份屏蔽了子类原型上的。。。

# 四、寄生组合继承（最佳）

```
function beget(obj){   // 生孩子函数 beget：龙beget龙，凤beget凤。
    return new F();
}
function Super(){
    // 只在此处声明基本属性和引用属性
    this.val = 1;
    this.arr = [1];
}
//  在此处声明函数
Super.prototype.fun1 = function(){};
Super.prototype.fun2 = function(){};
//Super.prototype.fun3...
function Sub(){
    Super.call(this);   // 核心
    // ...
}
var F = function(){};
F.prototype = Super.prototype;    //只改变F的原型对象指向，不创建实例
Sub.prototype = new F();
Sub.prototype.constructor = Sub;            // 核心           
```

# 五、寄生继承

# 六、原型继承

