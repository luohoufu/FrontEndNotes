### Element

在react中元素就是一个**对象**，**描述**一个组件实例或者dom节点需要的**属性**（组件类型，组件的属性，子元素），不能在这上面调用方法，**是一个不可修改的描述性对象**。

两个属性域（type：string或者reactClass；props：对象）

* DOM Element（当type属性为string时，代表一个标签名的DOM节点）	

```
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
其实表示的是下面的html结构
<button class='button button-blue'>
  <b>
    OK!
  </b>
</button>
```

* 组件 Element（当type属性是reactClass时，对应react的一个组件类）

```
{
  type: Button,
  props: {
    color: 'blue',
    children: 'OK!'
  }
}
```

描述一个组件的元素还是一个元素，更描述一个DOM节点的元素一样



### Component组件封装了元素树

react发现元素是组件A时，会根据props向A组件请求渲染的元素（实例化组件），A返回渲染的元素。也就是对于A组件，**props是其输入，而元素树就是输出**。

返回的元素树可以同时包含DOM元素和组件元素



### Component组件可以是类或函数

组件为类时，可以存储组件的状态和相应的DOM节点创建和销毁时的执行自定义的逻辑

函数相对于类就像一个是实现了render方法的类组件

**但是共同点就是输入是props，输出是元素树element**



### 自上而下的整合reconciliation

```
ReactDOM.render({
  type: Form,
  props: {
    isSubmitted: false,
    buttonText: 'OK!'
  }
}, document.getElementById('root'));
```

调用上述方法，react根据props向组件Form请求元素树，递归地整合出整棵元素树，

在react中整合的过程开始于ReactDom.render(),或者setState(),整合reconciliation的结束得到的是一颗元素树，最终react会利用diff计算出最小更新，再去更新真正的DOM。

这种渐进式的refine提炼过程，可以使react进行性能优化，比较state或者props就可以跳过这个整合reconciliation过程，不进行整合也就是不调用组件的render方法，从而提高性能。



[【参考1】](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

[【参考2】](http://www.oschina.net/translate/react-components-elements-and-instances)

