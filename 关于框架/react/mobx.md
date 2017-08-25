[TOC]

## 一、主要原理

autorun

```
const obj = observable({
  a:1,
  b:2
})
autoRun(() => {
  console.log(obj.a)
})
obj.b = 3;
obj.a = 2;//输出2
```

神奇的地方在于：**autorun绑定的是属性才回去改变，没有绑定的属性不会执行**

## 二、依赖收集

收集依赖，当变量改变时通过收集的依赖来判断是否需要更新

### 1.如何实现

### 存储结构

当变量修改后，确定哪些方法被触发，需要一个存储结构

* 存储所有的代理对象
* 存储代理对象属性的autorun的函数，也就是存储监听



## 三、核心概念

### 数据流

单向数据流，

![](https://pic3.zhimg.com/v2-7c7e6d63765099c40c9aabfebc23e712_b.png)

actions、state、computed values、reactions

通过事件驱动（UI事件、网络请求）触发actions，actions中修改了state（指的是store，数据存储）的值、计算computed values,反映到视图层。

### 可观察状态

当定义好其 observable 的对象值后，对象中后来添加的值是不会变为可观察的，这时需要使用 extendObservable 来扩展对象：

### 计算属性值



### 运行观察

修改state之后的监听是autorun。

autorun接受一个函数做参数，使用autorun的时候，当时运行一次，之后进行监听。

实际使用中，autorun中的函数就是用来操作reactions的，当可观察的状态属性的值发生改变的时候，可以在该函数中利用状态值修改UI视图。

**mobx-react 库则是封装了 autorun 用来在 store 中的可观察状态属性值发生改变的时候 rerender React 组件**



## 在react中的使用

