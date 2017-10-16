[TOC]

## void undefined的区别

**undefined和null是所有类型的子类型 **，undefined可以赋值给number类型的变量，

void类型的变量不能赋值给number类型的变量

**undefined和null只能被复制自身**

## 什么是never类型

## 下面代码会不会报错，怎么解决

```
const obj = {
  a:1,
  b:'string',
}

obj.c = null;
```

## readonly和const区别

## 下面代码中，foo的类型应该如何声明

```
function foo (a: number){
  return a+1
}
foo.bar = 123
```

```
let foo={};
for(let i=0; i< 100;i ++){
  foo[String(i)] = i;
}
```