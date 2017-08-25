数据界面驱动UI，一个数据模型的变化可能导致分散在界面多个角落的UI同时发生变化，界面越复杂，数据和界面的一致性越难维护，react初衷，**整体刷新界面，让框架去解决需要更新的问题，**

react实现了一层virtual DOM，映射原生DOM，通过这一层DOM避免react直接操作原生DOM，因为直接操作原声DOM的速度高于js对象，每当组件的props或state改变时，react会在内存中构造新的virtual DOM和旧的对比。

具有batching和高效diff，无需担心性能问题而毫无顾忌地随时刷新整个页面，不需要在意底层的dom操作，因为虚拟dom来确保对界面上真正变化的部分进行实际的DOM操作，

## 所谓virtual dom

不是真正的dom，只是js对象，因此称之为virtual dom

## jsx转换为js

* 在js中直接写dom不方便，jsx允许在在js中写html，也可以在{}中写js
* 需要一个函数将jsx转换为js对象，然后我们就可以将这个js对象作为真实DOM的输入
* 这个函数在react中就是react.createElement( )
* 但是将jsx转换成这个函数就需要babel了，babel便利每个jsx节点，将其转换为react.createElement( )函数调用
* 这个函数的输出就是virtualDOM

```
{
   "nodeName": "",
   "attributes": {},
   "children": []
}
```



### 初始化节点

* 为组件创建virtualDOM，如果是组件，实例化组件并且调用该组件的render（）方法返回virtualDOM，将virutualDOM扩展；若不是组件则创建真实DOM
* 循环递归创建子节点，并添加到父组件中（深度优先）



















