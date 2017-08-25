[TOC]



## 传统的diff算法



## react中的diff算法

首先，只会比较同一层级的diff，同一层级中的不同首先如果有id则对比id，没有id对比是否是相同的组件构成，或者比较是否为相同的dom结构。

diff策略

* dom节点跨层级移动操作比较少 tree diff
* 相同层级的子节点可以通过唯一id区分 element diff
* 拥有相同类的两个组件生成相似的树形结构，不同类不同 component diff



### tree diff

对树进行分层比较，只比较同层的节点。

![](https://pic1.zhimg.com/0c08dbb6b1e0745780de4d208ad51d34_b.png)

只对同一层级的节点进行比较，如果结点不存在了，直接删除重建，不会进行进一步的比较

跨层级的修改不会移动，会删除重建

这样会大大降低react的性能，重建需要时间，

> 在开发过程中保持稳定的dom结构有助于性能的提升，可以通过css隐藏或显示节点，而不是真的移除或者添加节点。

### component diff

* 同一个类型的组件，按照原策略继续比较
* 不同，替换整个组件
* 同一类型的组件，也可以使用shouldComponentUpdate来判断是否需要进行diff

### element diff

三种节点操作

* **INSERT_MARKUP**，新的 component 类型不在老集合里， 即是全新的节点，需要对新节点执行插入操作。
* **MOVE_EXISTING**，在老集合有新 component 类型，且 element 是可更新的类型，generateComponentChildren 已调用 receiveComponent，这种情况下 prevChild=nextChild，就需要做移动操作，可以复用以前的 DOM 节点。
* **REMOVE_NODE**，老 component 类型，在新集合里也有，但对应的 element 不同则不能直接复用和更新，需要执行删除操作，或者老 component 不在新集合里的，也需要执行删除操作。

## 总结

- React 通过制定大胆的 diff 策略，将 O(n3) 复杂度的问题转换成 O(n) 复杂度的问题；
- React 通过**分层求异**的策略，对 tree diff 进行算法优化；
- React 通过**相同类生成相似树形结构，不同类生成不同树形结构**的策略，对 component diff 进行算法优化；
- React 通过**设置唯一 key**的策略，对 element diff 进行算法优化；
- 建议，在开发组件时，保持稳定的 DOM 结构会有助于性能的提升；
- 建议，在开发过程中，**尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。**





[react源码diff算法](https://zhuanlan.zhihu.com/p/20346379)

