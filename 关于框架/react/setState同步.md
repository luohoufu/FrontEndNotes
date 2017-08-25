## 传入第二个参数

callback函数

## 传入函数作为参数

对象作为参数时，setState会将状态合并，类似Object.assign将对象合并之后，进行更新

函数作为参数时，setState将所有的更新排成一个队列，会按调用顺序进行setState

函数的参数是**旧state**和**旧props**

```javascript
　incrementCount(){
　　this.setState((prevState, props) => ({
　　count: prevState.count + 1
　　}));
　　this.setState((prevState, props) => ({
　　count: prevState.count + 1
　　}));
　}
```

### 最佳实践

分离action，增加组件的复用性，不用关心更新的方式。

将传入的函数放在组件外面，改写上述为

```Javascript
function add(prevState, props) {
  count: prevState.count + 1
}

incrementCount() {
  this.setState(add);
}
```