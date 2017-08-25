[TOC]

## 一、createStore

```
/**
 * 创建一个Redux store来存放应用所有的 state。应用中有且只有一个store
 *
 * @param {Function} reducer 是一个函数,接收两个参数，分别是当前的 state 树和
 * 要处理的 action，返回新的 state 树
 *
 * @param {any} 初始化时的state ,在应用中，你可以把服务端传来经过处理后的 state
 *传给它。如果你使用 combineReducers 创建 reducer，它必须是一个普通对象，与传入
 *的 keys 保持同样的结构。否则，你可以自由传入任何 reducer 可理解的内容。
 *
 * @param {Function} enhancer 是一个组合的高阶函数，返回一个强化过的 store creator .
 *                  这与 middleware相似，它也允许你通过复合函数改变 store 接口。
 *
 * @returns {Store} 返回一个对象，给外部提供 dispatch, getState, subscribe, replaceReducer, 
 */
export default function createStore(reducer, preloadedState, enhancer) {

  //判断 preloadedState 是一个函数并且 enhancer 是未定义 
  if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
    enhancer = preloadedState  // 把 preloadedState 赋值给 enhancer
    preloadedState = undefined // 把 preloadedState 赋值为 undefined
  }

  //判断 enhancer 不是 undefined
  if (typeof enhancer !== 'undefined') {
    //判断 enhancer 不是一个函数
    if (typeof enhancer !== 'function') {
      //抛出一个异常 (enhancer 必须是一个函数)
      throw new Error('Expected the enhancer to be a function.')
    }
    //调用 enhancer ,返回一个新强化过的 store creator
    return enhancer(createStore)(reducer, preloadedState)
  }
  
  //判断 reducer 不是一个函数
  if (typeof reducer !== 'function') {
    //抛出一个异常 (reducer 必须是一个函数)
    throw new Error('Expected the reducer to be a function.')
  }
```

## 二、store的方法

### getState（）

```
function getState() {
  return currentState;
}
```

### subscribe(listener)【为什么要设置nextlisteners？？】

*ensureCanMutateNextListeners()函数用来当前的nextListeners和currentListeners变量是一致的，同时因为Redux函数式的编程思维，使用slice函数来返回一个位于不同存储位置的新的listener数组对象，也就是使得nextListeners和currentListeners指向不同的对象。*？？？

添加变化监听器，每当dispatch action执行完后，会触发的函数

```
  /**
   * 保存一份订阅快照
   * @return {void}
   */
  function ensureCanMutateNextListeners() {
    //判断 nextListeners 和 currentListeners 是同一个引用
    if (nextListeners === currentListeners) {
      
      //通过数组的 slice 方法,复制出一个 listeners ,赋值给 nextListeners,此时nextListeners和current为不同引用
      nextListeners = currentListeners.slice()
    }
  }  
  /**
   *
   * 添加一个 listener . 当 dispatch action 的时候执行，这时 sate 已经发生了一些变化，
   * 你可以在 listener 函数调用 getState() 方法获取当前的 state
   *
   * 你可以在 listener 改变的时候调用 dispatch ，要注意
   *
   * 1. 订阅器（subscriptions） 在每次 dispatch() 调用之前都会保存一份快照。
   *    当你在正在调用监听器（listener）的时候订阅(subscribe)或者去掉订阅（unsubscribe），
   *    对当前的 dispatch() 外层的dispatch不会有任何影响（不会使用最新的订阅列表，因为dispath已经调用完值才会执行listener，此时已经不能改变外层的listener）。但是对于下一次的 dispatch()，无论嵌套与否，
   *    都会使用订阅列表里最近的一次快照。
   *
   * 2. 订阅器不应该关注所有 state 的变化，在订阅器被调用之前，往往由于嵌套的 dispatch()
   *    导致 state 发生多次的改变，我们应该保证所有的监听都注册 在 dispath() 之前。
   *
   * @param {Function} 要监听的函数
   * @returns {Function} 一个可以移除监听的函数  在dispatch内部执行
   */
  function subscribe(listener) {
	...
	
    //标记有订阅的 listener
    var isSubscribed = true

    //保存一份快照nextListeners 独立
    ensureCanMutateNextListeners()

    //添加一个订阅函数
    nextListeners.push(listener)
    
    //返回一个取消订阅的函数
    return function unsubscribe() {

      //判断没有订阅一个 listener
      if (!isSubscribed) {
        return
      }

      //标记现在没有一个订阅的 listener
      isSubscribed = false
      
      //保存一下订阅快照
      ensureCanMutateNextListeners()
      
      //找到当前的 listener在数组中的index 移除当前的 listener
      var index = nextListeners.indexOf(listener)
      nextListeners.splice(index, 1)
    }
  }
```

### dispatch(action)

分发action，改变state的唯一方法

```
/**
   * dispath action。这是触发 state 变化的惟一途径。
   * 
   * @param {Object} 一个普通(plain)的对象，对象当中必须有 type 属性
   *
   * @returns {Object} 返回 dispatch 的 action
   *
   * 注意: 如果你要用自定义的中间件， 它可能包装 `dispatch()`
   *       返回一些其它东西，如( Promise )
   */
  function dispatch(action) {
    //判断 action 不是普通对象。也就是说该对象由 Object 构造函数创建
    if (!isPlainObject(action)) {
      throw new Error(
        'Actions must be plain objects. ' +
        'Use custom middleware for async actions.'
      )
    }
    
	...
	
    try {
      //标记 dispatch 正在运行，执行当前 Reducer 函数返回新的 state
      isDispatching = true
      currentState = currentReducer(currentState, action)
    } finally { 
      isDispatching = false
    }

    //遍历执行所有的监听函数
    var listeners = currentListeners = nextListeners
    for (var i = 0; i < listeners.length; i++) {
      listeners[i]()
    }

    //返回传入的 action 对象
    return action
  }

```

### replaceReducer(nextReducer)

```
/**
   * 替换计算 state 的 reducer。
   *
   * 这是一个高级 API。
   * 只有在你需要实现代码分隔，而且需要立即加载一些 reducer 的时候才可能会用到它。
   * 在实现 Redux 热加载机制的时候也可能会用到。
   *
   * @param {Function} store 要用的下一个 reducer.
   * @returns {void}
   */
  function replaceReducer(nextReducer) {

    //判断 nextReducer 不是一个函数
    if (typeof nextReducer !== 'function') {
      throw new Error('Expected the nextReducer to be a function.')
    }
    currentReducer = nextReducer
    
    //调用 dispatch 函数，传入默认的 action
    dispatch({ type: ActionTypes.INIT })
  }
```

## combineReducers(reducers)

对reducer有一些要求

* 未匹配到的action，必须把接收到的第一个参数返回
* 永远不能返回undefined
* 初始state禁用undefined

```
export default function combineReducers(reducers) {
  const reducerKeys = Object.keys(reducers)
  const finalReducers = {}
  for (let i = 0; i < reducerKeys.length; i++) {
    const key = reducerKeys[i]

    if (process.env.NODE_ENV !== 'production') {
      if (typeof reducers[key] === 'undefined') {
        warning(`No reducer provided for key "${key}"`)
      }
    }

    if (typeof reducers[key] === 'function') {
      finalReducers[key] = reducers[key]
    }
  }
  const finalReducerKeys = Object.keys(finalReducers)

  let unexpectedKeyCache
  if (process.env.NODE_ENV !== 'production') {
    unexpectedKeyCache = {}
  }

  let shapeAssertionError
  try {
    assertReducerShape(finalReducers)
  } catch (e) {
    shapeAssertionError = e
  }

  return function combination(state = {}, action) {
    if (shapeAssertionError) {
      throw shapeAssertionError
    }

    if (process.env.NODE_ENV !== 'production') {
      const warningMessage = getUnexpectedStateShapeWarningMessage(state, finalReducers, action, unexpectedKeyCache)
      if (warningMessage) {
        warning(warningMessage)
      }
    }

    let hasChanged = false
    const nextState = {}
    for (let i = 0; i < finalReducerKeys.length; i++) {
      const key = finalReducerKeys[i]
      const reducer = finalReducers[key]
      const previousStateForKey = state[key]
      //获得子reducer获取的新的state，reducer必须问纯函数，不改变传入的参数
      const nextStateForKey = reducer(previousStateForKey, action)
      if (typeof nextStateForKey === 'undefined') {
        const errorMessage = getUndefinedStateErrorMessage(key, action)
        throw new Error(errorMessage)
      }
      //新的state赋值给nextstate对应的key
      nextState[key] = nextStateForKey
      //判断是否改变
      hasChanged = hasChanged || nextStateForKey !== previousStateForKey
    }
    return hasChanged ? nextState : state
  }
}

```

## 三、bindActionCreators(actionCreators,dispatch)？？？

把 [action creators](http://cn.redux.js.org/docs/Glossary.html#action-creator) 转成拥有同名 keys 的对象，但使用 [`dispatch`](http://cn.redux.js.org/docs/api/Store.html#dispatch) 把每个 action creator 包围起来，这样可以直接调用它们。

*你或许要问：为什么不直接把 action creators 绑定到 store 实例上，就像传统 Flux 那样？问题是这样做的话如果开发同构应用，在服务端渲染时就不行了。多数情况下，你 每个请求都需要一个独立的 store 实例，这样你可以为它们提供不同的数据，但是在定义的时候绑定 action creators，你就可以使用一个唯一的 store 实例来对应所有请求了*？？？

当你需要把actionCreator传递给子组件，但又不想传递store或者dispatch

## 四、applyMiddleware

例子：

```
let store = createStore(
  todos,
  [ 'Use Redux' ],
  applyMiddleware(logger)
)

自定义middleware的写法
function logger({getState}){
  return (next) => (action) => {
    let returnValue = next(action)//循环递归调用后面的中间件，并传递action参数(参数的执行结果)f(g(h(action)))
    return returnValue
  }
}
```

```
  在createStore.js中
  //判断 enhancer 不是 undefined
  if (typeof enhancer !== 'undefined') {
    //判断 enhancer 不是一个函数
    if (typeof enhancer !== 'function') {
      //抛出一个异常 (enhancer 必须是一个函数)
      throw new Error('Expected the enhancer to be a function.')
    }
    //调用 enhancer ,返回一个新强化过的 store creator
    return enhancer(createStore)(reducer, preloadedState)
  }

export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    const store = createStore(reducer, preloadedState, enhancer)
    let dispatch = store.dispatch
    let chain = []

    const middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    }
    //对各个中间件的store变量赋值，中间件获得{State，dispatch}参数，由于闭包（return 函数）的原因，中间件执行过程中可以访问这两个参数
    //执行过之后的（next）=> (action)={//可以使用		   store.getState()store.dispatch(action)}
    chain = middlewares.map(middleware => middleware(middlewareAPI))
    //对中间件的next赋值形成中间件串联
    dispatch = compose(...chain)(store.dispatch)

    return {
      ...store,
      dispatch    //覆盖dispatch
    }
  }
}
```

### compose

```
export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}

```

### 洋葱模型

一直向下执行，执行到最后的时候，next即为dispatch(action)，

![](https://pic3.zhimg.com/v2-e5b8f433fec45c09260759fb12e90bb6_b.png)yic