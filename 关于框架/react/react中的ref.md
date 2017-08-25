ref代表对组件或者节点的引用，其实就是ReactDom.render()返回的组件实例，渲染组件，返回的是组件实例，渲染dom节点返回的事dom元素

## ref设置为回调函数

* 组件被挂载之后，回调函数被立即执行
* 组件卸载之后，回调函数也会执行

```
ReactDOM.findDOMNode(ref) 获取dom节点
```

## ref无状态组件的使用

```
function TestComp(props){
    let refDom;
    return (<div>
        <div ref={(node) => refDom = node}>
        </div>
    </div>)
}
```