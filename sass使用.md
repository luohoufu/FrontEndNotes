[TOC]

## 变量

```
$blue:red;
div{
  color: $blue;
}
$side:left;
div{
   border-#{$side}-radius: 5px;      放在属性键名中使用#{$side}
}
```

## 计算

```
body{
  margin:(14px/2)
  top: 50px + 100px;   $var*!)%
}
```

## 嵌套

```
属性也可以嵌套
p{
  border:{ 带冒号注意
    color: red;
  }
}

引用父元素
a{
	&:hover{
      color:red;
	}
}
```

## 代码复用

### .class @extend

```
.class1 {}
.class2{
  @extend .class1;
  font-size: 120px;
}
```

### @mixin @include

```
@mixin left($value: 10px){
  float: left;
  margin-left: 10px;
}
div{
  @include left(20px);
}
生成浏览器前缀实例
@mixin rounded($vert, $horz, $radius: 10px) {
　　　　border-#{$vert}-#{$horz}-radius: $radius;
　　　　-moz-border-radius-#{$vert}#{$horz}: $radius;
　　　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
　　}
}
```

### %name @extend

```
%class1 {}
.class2{
  @extend .class1;
  font-size: 120px;
}
```

|      | 混合宏                      | 继承                           | 占位符          |
| ---- | ------------------------ | ---------------------------- | ------------ |
| 声明方式 | @mixin                   | .class                       | %name        |
| 调用方式 | @include                 | @extend                      | @extend      |
| 使用环境 | 可传入参数／编译出来的代码每次调用都会加载，冗余 | 不支持传入参数／不管调不调用都会有样式代码／基类只有一个 | 占位符不调用不会产生代码 |

## 条件语句

```
p{
	@if 1 + 1 == 2 {}
	@if 5<3 {}
}
　　@if lightness($color) > 30% {
　　　　background-color: #000;
　　} @else {
　　　　background-color: #fff;
　　}

```

## 循环语句

```
@for $i from 1 to 10 {
  .border-#{$i} {
    border: #{$i}px solid blue;
  }
}

$i: 6;
　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}
```

## 自定义函数 

**返回的是一个值**

```
@function double($n) {
　　　　@return $n * 2;
　　}
　　#sidebar {
　　　　width: double(5px);
　　}

```