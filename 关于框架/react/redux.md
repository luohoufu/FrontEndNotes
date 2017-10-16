[TOC]

## WHY

* 用户的使用方式复杂
* 不同身份的用户有不同的使用方式
* 多个用户之间可以协作
* 与服务器大量交互，使用websocket
* view从多个来源获取数据



组件

* 某个组件的状态需要共享
* 某个状态需要在任何地方都可以拿到
* 一个组件需要改变全局状态
* 一个组件需要改变另一个组件的状态

## WHAT

所有的状态，保存在一个对象里面。

## HOW（源码）

## 基本概念和API

### store

整个应用一个store

生成store

```
import {createStore} from 'redux';
const store = createStore(fn);//函数参数，返回一个store对象
```

### state

store某个时间点的快照，state

```
import {createStore} from 'redux';
const store = createStore(fn);
const state = store.getState();
```

state对应view

### action

view要改变state，就必须要用action改变state

### action creator

使用函数生成action

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
const action = addTodo('hhhh')
```

### store.dispatch()

view发出action的唯一方法，

```
store.dispatch(addTodo('Learn Redux'));
```

### reducer 函数

store收到action之后，必须给出一个新的state，这样view才会发生变化，这种state的计算过程叫做reducer。

接受state和action

```
const reducer = (state=defaultState, action) => {
  switch(action.type) {
    case:'add':
    	return state + action.payload;
    default:
    	return state;
  }
}
```

实际reducer函数不需要手动调用，store.dispatch方法触发reducer的自动执行，因此

const store = createStore(reducer);

* 纯函数，同样的输入得到**同样的输出， 不改变参数**
* state设置为只读，返回**新对象**

```
return Object.assign({}, state, { thingToChange });
  // 或者
  return { ...state, ...newState };
  
  //数组
  return [...state, newItem];
```

### store.subscribe()

store允许使用store.subscribe方法设置监听函数，一旦state发生变化，就自动执行这个函数。

对于react，把组件的render方法或者setState方法放入listen，就会实现view的自动渲染。

**store.subscribe 方法返回一个函数，调用函数就可以解除监听**

```
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```

### reducer拆分

```
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
```