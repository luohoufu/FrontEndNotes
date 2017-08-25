# 关于虚拟DOM

DOM很慢，页面的很多性能问题，前端主要工作是维护状态和更新视图，都需要DOM操作，所以解放DOM操作。

## 理解虚拟DOM

虚拟的DOM的核心思想是：对复杂的文档DOM文档，提供一种方便的工具，进行最小化的DOM 操作

* 提供一种方便的工具，使得开发效率得到保证.pro
* 保证最小化的DOM操作，使得执行效率得到保证

### 1.js表示DOM结构

dom很慢，js很快，用js对象可以很容易地表示dom节点，dom节点包括标签属性字节点，通过 Velement表示

```
标签名	属性 子孩子 
var VElement = function(tagName, props, children){
  if(!(this instanceof VElement)) {
    return new VElement(tagName, props, children);
  }
  if(util.isArray(props)) {
    children = props;
    props = {};
  }
   //设置虚拟dom的相关属性
    this.tagName = tagName;
    this.props = props || {};
    this.children = children || [];
    this.key = props ? props.key : void 666;
    var count = 0;
    util.each(this.children, function(child, i) {
        if (child instanceof VElement) {
            count += child.count;
        } else {
            children[i] = '' + child;
        }
        count++;
    });
    this.count = count;
}
```

因此我们可以简单地用js表示dom结构，如下：

```
var vdom = velement('div', { 'id': 'container' }, [
    velement('h1', { style: 'color:red' }, ['simple virtual dom']),
    velement('p', ['hello world']),
    velement('ul', [velement('li', ['item #1']), velement('li', ['item #2'])]),
]);
```

```
<div id="container">
    <h1 style="color:red">simple virtual dom</h1>
    <p>hello world</p>
    <ul>
        <li>item #1</li>
        <li>item #2</li>
    </ul>   
</div>
```

我们可以构造一个方法，根据虚拟dom节点递归地构造出真实的DOM树

```
VElement.prototype.render = function() {
    //创建标签
    var el = document.createElement(this.tagName);
    //设置标签的属性
    var props = this.props;
    for (var propName in props) {
        var propValue = props[propName]
        util.setAttr(el, propName, propValue);
    }
    //依次创建子节点的标签
    util.each(this.children, function(child) {
        //如果子节点仍然为velement，则递归的创建子节点，否则直接创建文本类型节点
        var childEl = (child instanceof VElement) ? child.render() : document.createTextNode(child);
        el.appendChild(childEl);
    });

    return el;
}
```

对于一个虚拟的dom对象的VElement，调用其原型的render方法，就可以产生一颗真实的DOM树

那么当数据状态发生变化需要改变DOM结构式，我们通过js对象表示的**虚拟DOM计算出实际dom需要做的最小变动**，然后再操作实际DOM，从而避免了粗放式的DOM操作带来的性能问题



# vue数据绑定和响应式原理

当实例化一个vue构造函数时，会执行vue的init方法，在init方法中主要执行三部分内容

* 初始化环境变量
* 处理vue组件数据
* 解析挂载组件

vue实现了观察者-订阅者模式实现数据驱动视图，通过设定对象属性的setter/getter方法来监听数据的变化，每个属性的setter方法就是一个观察者，属性发生变化将会向订阅者发送消息，从而驱动视图更新

vue的订阅者watcher是现在watcher.js中，构建有一个watcher最重要的是expOrFn和cb两个参数，cb是订阅者收到的消息后执行的回调（一般是视图指令的更新方法），expOrFn的getter方法，从而间接获得订阅的对象属性

# 数据订阅

vue的数据发生在生命周期init和created之间执行