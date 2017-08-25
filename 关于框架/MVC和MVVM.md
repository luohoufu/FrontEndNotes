![](http://pic002.cnblogs.com/images/2011/24476/2011110417301778.jpg)

## MVC   model view controller

model 数据

view UI组件，负责展示从controller得到的数据，也是model到UI的过程

在MVC模式中，除了Controller可以访问Model， **View也允许直接访问Model（Model不依赖View，但是View依赖Model）**。因此，View中可能含有一些业务逻辑，导致View的可重用性降低

## MVP model view processor

**Presenter**
它负责处理View上各类UI事件。如图所示，View直接面对User，通过View，Presenter接受输入原数据，然后交给Model处理，再把最终结果展示在View上。
Presenter与View通过定义好的接口交互，是一种低耦合模式。

MVP与MVC最大的不同，在于Model和View完全隔离开，两者必须通过Presenter进行通信。因此，主要业务处理都放在了Presenter层，View层变得比较薄弱。
MVP模式下，表现层和数据层分开，方便单元测试。

## MVVM model view viewmodel

**View Model**
它暴露了一系列的方法，命令，或者属性，用于帮助维护View状态，操纵Model数据并最终作用在View上。
支持View和ViewModel的双向数据绑定。

MVVM是MVP的演化版本，在概念上真正将页面和数据逻辑分开。
它最大的特点就是双向绑定(data-binding)：View改变，ViewModel自动更新；ViewModel更新，Model同步改变。反之亦然。
一般，ViewModel中的属性都实现了一些监听器/观察器，用于View或者Model的同步刷新。
大多数情况，MVVM模式需要依赖具体的平台或者技术实现，比如Vue.js。

**state对应model，UI对应view**

**UI = F(state)**





