[TOC]



## what is typescript

是js的一个超集，主要提供类型系统和es6的支持，可以编译成plain js

由microsoft开发，开源

## why typescript（？？？）

* 增加代码的可读性和可维护性 （类型系统、编译阶段发现错误、增强编辑器和IDE的功能（代码补全、接口提示、跳转到定义、重构等））
* 不显式的定义类型，可以自动做出类型推论，编译报错也可以生成js文件、第三方库不是ts写的，也可以编写单独的类型文件供ts读取



需要理解接口、泛型、类型、枚举类型等概念



# 基础

## 一、原始数据类型

### 1.基本类型和构造函数不同

```
let createdByNewBoolean: boolean = new Boolean(1);

// index.ts(1,5): error TS2322: Type 'Boolean' is not assignable to type 'boolean'.
// 后面约定，未强调编译错误的代码片段，默认为编译通过

事实上 new Boolean() 返回的是一个 Boolean 对象：
let createdByNewBoolean: Boolean = new Boolean(1);

直接调用 Boolean 也可以返回一个 boolean 类型：
let createdByBoolean: boolean = Boolean(1);
```

### 2.数值

```
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;  编译后会变为数字  es6增加的二进制和八进制表示法
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

### 3.空值 ：void

### 4.null undefined

undefined和null是所有类型的子类型，undefined类型的变量可以赋值给其他类型，void不可

## 二、任意值

设置为any，允许被赋值为任意类型，对他的任何操作，返回的内容的类型都是any

访问任意值的属性和方法都是允许的不会报错

未指定类型，识别为任意类型

## 三、类型推论

如果**定义时赋值**，则会将进行类型推断

```
let something = 'seven';
something = 7;
报错

let something;
something = 'seven'
something = 7
不会报错
```

## 四、联合类型

取值可以为多种类型的一种

```
let number = string | number
```

只能访问联合类型的所有类型里**共有**的属性或方法

被赋值后，会根据类型推论的规则推断类型

```
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

## 五、对象的类型--接口

使用接口定义对象的类型

```
interface Person {
  readonly id:number; //只读属性
  name: string;
  age？: number; //可选属性
  [propName:string]: any;  //任意属性
}

let item: Person = {
  id: 123,
  name: 'Xcat Liu',
  age: 25,
  website: 'http://xcatliu.com',
};

item.id = 9527; //报错

item会被推断为类型如下  会为确定属性和可选属性的联合类型
{ 
  [x: string]: string | number; 
  name: string; 
  age: number; 
  website: string; 
  }
```

**一旦定义了任意属性，确定属性和可选属性必须是它的子属性**

**只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**

## 六、数组

```
let array: (number | string)[] = [1,2,3];
array.push('8')
```

数组泛型

```
let array: Array<number> = [1,1,2,3,5]
```

接口表示

```
interface NumberArray {
  [index: number]: number;
}
```

any在数组中

```
let array: any[] = []数组中可以出现各种类型的元素
```

类数组  常见的类数组都有自己的接口定义，如IArguments，NodeList，HTMLCollection

## 七、函数

函数声明

```
function sum(x: number, y: number): number {
  return x + y;
}
sum(1, 2, 3);//参数不同报错
```

### 1.函数表达式（？？？）

```
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
  return x + y;
};
```

### 2.函数的接口

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  return source.search(subString) !== -1;
}
```

### 3.可选参数

**可选参数后面不允许在出现必须参数**

```
let mySum: (x: number, y?: number) => number = function (x: number, y: number): number {
  return x + y;
};
```

### 4.参数默认值

添加默认值的参数识别为可选参数，且不受可选参数必须在必须参数后面的限制

```
function buildName(firstName: string = 'Xcat', lastName: string) {
  return firstName + ' ' + lastName;
}
```

### 5.剩余参数

```
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
  });
}

function push(array: any[], ...items: any[]) {
  items.forEach(function(item) {
    array.push(item);
  });
}

let a = [];
push(a, 1, 2, 3);
```

### 6.重载（和泛型？？？）

一个函数接受不同数量和类型的参数时，做出不同的处理。

重复定义，最后一次实现

```
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
}
```

### 7.函数接口（？？？）

```
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## 类型断言

可以绕过编译器的类型推断，手动指定一个值的类型

```
<type>值
```

断言类型不是类型转换，断言成一个联合类型中的不存在的类型是不允许的。

```
function toBoolean(something: string | number): boolean {
  return <boolean>something;//报错
}
```

## 声明文件

使用第三方库时，使用declare关键字来定义它的类型

```
declare var jQuery: (string) => any;
jQuery('#foo');
```

```
// jQuery.d.ts声明文件
declare var jQuery: (string) => any;

三斜杠表示引入声明文件
/// <reference path="./jQuery.d.ts" />
jQuery('#foo');
```

# 进阶

## 一、类型别名&&字符串字面量类型

```
type Name = string;
type NameResolver = () => string //也可以是函数
function getName(n: NameOrResolver): Name {
  if(typeof n === 'string') {
    return n;
  }
  else {
    return n();
  }
}

type EventNames = 'click' | 'scroll' | 'mousemove';
event: EventNames //是其中之一
```

使用type创建类名别名和字符串字面量类型

## 二、元组

```
let array: [string, number] = ['hh',25]
可以分别赋值
array[0] = 0;
array[1] = 0
初始化时需要提供全部指定项

元祖指定元素赋值越界 赋值魏联合类型 string | number
访问一个越界的元素，也会识别为元祖中每个类型的联合类型
let item: [string, number];
xcatliu = ['Liu', 25, 'http://xcatliu.com/'];

console.log(item[2].slice(1));//报错，只能访问联合类型共有的属性和方法

```

## 三、枚举

枚举成员被赋值魏从0开始递增的数字和值双向映射

```
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days[0] === "Sun"); // true


编译为
var Days;
(function (Days) {
  Days[Days["Sun"] = 0] = "Sun";
  Days[Days["Mon"] = 1] = "Mon";
  Days[Days["Tue"] = 2] = "Tue";
  Days[Days["Wed"] = 3] = "Wed";
  Days[Days["Thu"] = 4] = "Thu";
  Days[Days["Fri"] = 5] = "Fri";
  Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));

可以手动赋值，未赋值的枚举项会接着上一个枚举项递增
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat}

```

**可能会出现覆盖的情况**

**可以不是数字，可以使用类型断言来让tsc无视类型检查**

```
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

**可以使小数负数，后面递增步长为1**

## 四、类

public private和protected

* public 在任何地方都可以被访问
* private 只能在声明的类中访问，子类中也不能访问
* protected修饰的属性和方法是受保护的，和private类似，区别**子类允许访问**

```
class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}
// index.ts(11,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.

class Animal {
  protected name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}
```

### 1.抽象类

abstract用于定义抽象类和其中的抽象方法

抽象类不允许被实例化，只能被继承

```
abstract class Animal { 
}
let a = new Animal();//报错

class Cat extends Amimal {
  public ca
}
```

**抽象类的抽象方法必须被子类实现**

**即使是抽象类，编译之后会存在这个类**

### 2.类的类型

给类加上类型

```
let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

## 五、类与接口(有用么???)

不同的类会有**共有的一些特性**，这时候可以把特性**提取成接口**，用implements关键字来实现

```
interface Alarm {
  alert();
}
class Door {
}

class SecurityDoor extends Door implements Alarm {
  alert() {    
  }
}
class Car implements Alarm {  
}
```

### 1.接口继承类

```
class Point {
  x: number;
  y: number;
}
interface poi extends Point {
  z: number;
}
let point3d: Point3d = {x: 1, y: 2, z: 3};
```



## 六、泛型

指在定义函数、接口和类的时候，**不预先指定具体的类型，而是在使用的时候在指定类型**的一种特性。

**在函数名后添加了<T>** ,T用来只带任意输入的类型，在后面的输入value: T和输出Array<T>中即可使用了，调用的时候，可以指定具体的类型为string，也可以不手动指定，让类型推论自动推算出来

```
function createArray<T>(length:number, value: T):Array<T> {
  let result = [];
  for(let i=0;i < length;i++) {
  }
}
createArray<string>(3, 'x');
```



### 1.多个类型参数

```
fucntion swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}
swap([7,'seven'])
```



### 2.泛型约束

函数内部使用泛型变量的时候，由于不知道是那种类型，所以不能随意的操作它的属性或方法 ，可以对泛型进行约束，只允许这个函数传入那些包含length属性的变量。

```
interface Lengthwise {
  length: number;
}
//extends 约束了泛型T必须符合接口Lengthwise的形状，也就是必须包含length属性
function getSome<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
//多个参数类型也可以相互约束
function copyFields<T extends U, U>(target: T,source: U): T {
  
}
```

### 3.泛型接口

可以使用接口的方式来定义一个函数需要符合的形状(参数，返回值类型)：

也可以使用**带有泛型的接口**定义函数的形状

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFUnc;//指定函数的参数返回值类型
mySearch = function(source: string, subString: string) {
  return source.search(subString) !== -1;
}

//泛型接口
interface CreateArrayFunc {
  <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
}

//还可以将泛型提到接口上
interface createArrayFunc<T> {}
```

### 4.泛型类

```
class GenericNumber<T> {
  zeroValue: T;
  add: (x:T,y:T) => T;
}
```