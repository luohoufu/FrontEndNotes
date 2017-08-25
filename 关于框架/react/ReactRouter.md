API：

### router

有三种，分别使用

* BrowserRouter
* HashRouter
* MemoryRouter

### Link

replace改为替换

```
<Link to="/about">关于</Link>
```

源码：

```
// prop参数
static propTypes = {
    onClick: PropTypes.func,
    target: PropTypes.string,	//a标签中的target
    replace: PropTypes.bool,	//是否调用history.replace
    to: PropTypes.oneOfType([   //href  可以是一个url，也可以是一个location对象
      PropTypes.string,
      PropTypes.object
    ]).isRequired
}

handleClick = (event) => {
  if (this.props.onClick)
    this.props.onClick(event)

  if (
    !event.defaultPrevented && // 是否阻止了默认事件
    event.button === 0 && // 确定是鼠标左键点击
    !this.props.target && // 避免打开新窗口的情况
    !isModifiedEvent(event) // 无视特殊的key值，是否同时按下了ctrl、shift、alt、meta
  ) {
    event.preventDefault()

    const { history } = this.context.router
    const { replace, to } = this.props

    if (replace) {
      history.replace(to)   //使用replacestate方法
    } else {
      history.push(to)		//使用pushstate方法
    }
  }
}
```

- link最终渲染成a，
- 点击之后，history包（用来兼容不同浏览器的历史记录的封装的包）中调用底层的history.pushstate方法传入path和state，在其中调用createLocation方法，返回location 的结构；再通过transitionTo将location转化为原生history调用方法，window.location.hash 或者window.history.pushState() 修改URL
- 触发history.listen的监听器（**在router组件中componentWillMount 生命周期方法中调用了 history.listen(listener) 方法**），根据生成的location，**matchRoutes**匹配route组件树与当前**location对象**匹配的一个子集，于是得到**nextstate**，接下来**执行 this.setState(nextState) **，就可以实现重新渲染 Router 组件

```
nextState = {
  location, // 当前的 location 对象
  routes, // 与 location 对象匹配的 Route 树的子集，是一个数组
  params, // 传入的 param，即 URL 中的参数
  components, // routes 中每个元素对应的组件，同样是数组
};
```



### Redirect

将当前的route重定向到另外一个URL，覆盖原地址（可以通过push来修改，插入到history中）

```
import { Route, Redirect } from 'react-router'


<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard"/>
  ) : (
    <PublicHomePage/>
  )
)}/>


源码
class Redirect extends React.Component {
  //...省略一部分代码

  isStatic() {
    return this.context.router && this.context.router.staticContext
  }

  componentWillMount() {
    if (this.isStatic())
      this.perform()
  }

  componentDidMount() {
    if (!this.isStatic())
      this.perform()
  }

  perform() {
    const { history } = this.context.router
    const { push, to } = this.props

    if (push) {
      history.push(to)
    } else {
      history.replace(to)
    }
  }

  render() {
    return null  没有内容
  }
}
```

### Route 

path没有默认渲染

当location匹配路由的path时，渲染UI

```
import { BrowserRouter as Router, Route } from 'react-router-dom'

<Router>
  <div>
    <Route exact path="/" component={Home}/>
    <Route path="/news" component={NewsFeed}/>
  </div>
</Router>
```

#### 1.三种渲染方法（什么区别？）

* component
* render

```
<Route path="/home" render={() => <div>Home</div>}/>
```

* children

不管地址是否匹配都渲染一些内容

```
<Route children={({ match, ...rest }) => (
  {/* Animate总会被渲染, 所以你可以使用生命周期来使它的子组件出现
    或者隐藏
  */}
  <Animate>
    {match && <Something {...rest}/>}
  </Animate>
)}/>
```

#### 2.渲染之后的props

* match

>参数
>
>**Route的path中冒号":"后的部分相当于通配符，而匹配到的url将会把匹配的部分作为match.params中的属性传递给组件，属性名就是冒号后的字符串。**
>
>- params` -（ object 类型）即路径参数，通过解析URL中动态的部分获得的键值对。
>- `isExact` - 当为 `true` 时，整个URL都需要匹配。
>- `path` -（ string 类型）用来做匹配的路径格式。在需要嵌套 `<Route>` 的时候用到。
>- `url` -（ string 类型）URL匹配的部分，在需要嵌套 `<Link>` 的时候会用到。
>
>使用方式
>
>- 在 [Route component](https://reacttraining.cn/web/api/Route/component) 中，以 `this.props.match` 方式。
>- 在 [Route render](https://reacttraining.cn/web/api/Route/render-func) 中，以 `({ match }) => ()` 方式。
>- 在 [Route children](https://reacttraining.cn/web/api/Route/children-func) 中，以 `({ match }) => ()` 方式
>- 在 [withRouter](https://reacttraining.cn/web/api/withRouter) 中，以 `this.props.match` 方式
>- [matchPath](https://reacttraining.cn/web/api/withRouter) 的返回值

* location

>获取location
>
>- 在 [Route component](https://reacttraining.cn/web/api/Route/component) 中，以 `this.props.location` 的方式获取，
>- 在 [Route render](https://reacttraining.cn/web/api/Route/render-func) 中，以 `({ location }) => ()` 的方式获取，
>- 在 [Route children](https://reacttraining.cn/web/api/Route/children-func) 中，以 `({ location }) => ()` 的方式获取，
>- 在 [withRouter](https://reacttraining.cn/web/api/withRouter) 中，以 `this.props.location` 的方式获取。

**location对象不会发生改变，因此可以在生命周期的钩子函数中使用location来判断当前页面是否发生了改变**

```
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // 已经跳转了！
  }
}

{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

> 可以使用对象的方式使用location，并且当你需要多个 UI ，而这些 UI 取决于历史时，例如弹出框（modal），使用location 对象会有很大帮助。



* history

  > 术语
  >
  > - 「browser history」 - history 在 DOM 上的实现，经常使用于支持 HTML5 history API 的浏览器端。
  > - 「hash history」 - history 在 DOM 上的实现，经常使用于旧版本浏览器端。
  > - 「memory history」 - 一种存储于内存的 history 实现，经常用于测试或是非 DOM 环境（例如 React Native）。
  >
  > 属性和方法
  >
  > - `length` -（ number 类型）指的是 history 堆栈的数量。
  > - `action` -（ string 类型）指的是当前的动作（action），例如 `PUSH`，`REPLACE` 以及 `POP` 。
  > - `location` -（ object类型）是指当前的位置（location），location 会具有如下属性：`pathname` -（ string 类型）URL路径。`search` -（ string 类型）URL中的查询字符串（query string）。`hash` -（ string 类型）URL的 hash 分段。`state` -（ string 类型）是指 location 中的状态，例如在 `push(path, state)` 时，state会描述什么时候 location 被放置到堆栈中等信息。这个 state 只会出现在 browser history 和 memory history 的环境里。
  > - `push(path, [state])` -（ function 类型）在 hisotry 堆栈顶加入一个新的条目。
  > - `replace(path, [state])` -（ function 类型）替换在 history 堆栈中的当前条目。
  > - `go(n)` -（ function 类型）将 history 对战中的指针向前移动 `n` 。
  > - `goBack()` -（ function 类型）等同于 `go(-1)` 。
  > - `goForward()` -（ function 类型）等同于 `go(1)` 。
  > - `block(prompt)` -（ function 类型）阻止跳转，（请参照 [history 文档](https://github.com/ReactTraining/history#blocking-transitions)）。

  **history可以改变，因此不能通过this.props.history.location获得location**

#### 3.path

url 没有的匹配所有

#### 4.exact

值为true时，path与location.pathname 必须完全匹配

| 路径   | location.pathname | exact | 是否匹配 |
| ---- | ----------------- | ----- | ---- |
| ／one | ／one／two          | true  | 否    |
| ／one | ／one／two          | false | 是    |

#### 5.strict

有结尾斜线的路径只能匹配有斜线的location.pathname

#### 6.源码

```
class Route extends Component {
  static propTypes: {
    path: PropTypes.string,
    exact: PropTypes.bool,
    component: PropTypes.func,
    render: PropTypes.func,
  }
//将要mount时，添加popstate事件监听
  componentWillMount() {
    addEventListener("popstate", this.handlePop)
  }

  componentWillUnmount() {
    removeEventListener("popstate", this.handlePop)
  }
//触发popstate forceupdate强制刷新
  handlePop = () => {
    this.forceUpdate()
  }

  render() {
    const {
      path,
      exact,
      component,
      render,
    } = this.props

    //location是一个全局变量
    const match = matchPath(location.pathname, { path, exact })

    return (
      //有趣的是从这里我们可以看出各属性渲染的优先级，component>render>children
      component ? (
        match ? React.createElement(component, props) : null
      ) : render ? ( // render prop is next, only called if there's a match
        match ? render(props) : null
      ) : children ? ( // children come last, always called
        typeof children === 'function' ? (
          children(props)
        ) : !Array.isArray(children) || children.length ? ( // Preact defaults to empty children array
          React.Children.only(children)
        ) : (
              null
            )
      ) : (
              null
            )
    )
  }
}
```



## route matching 

### 路径表达式

- `:paramName` – 匹配参数直到 `/`, `?`, or `#`.
- `()` – 匹配可选的路径
- `*` – 非贪婪匹配所有的路径
- `**` - 贪婪匹配所有字符直到 `/`, `?`, or `#`

```
<Route path="/hello/:name">         // 匹配 /hello/michael and /hello/ryan
<Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael, and /hello/ryan
<Route path="/files/*.*">           // 匹配 /files/hello.jpg and /files/hello.html
<Route path="/**/*.jpg">            // 匹配 /files/hello.jpg and /files/path/to/file.jpg
```



## route源码

```

```



