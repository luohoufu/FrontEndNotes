[TOC]

## 一、WHY?

将所有界面包括第三方的文件加载再同一个文件中虽然可以减少http请求，但是这样可能加载不必要的代码。比如第一个页面没有用到的功能代码。所以codeSpliting将代码分成多个chunk

## 二、HOW？

* 分离第三方代码
* 按需加载

## 三、分离vendor第三方（提取公共模块）

```javascript
module.exports = {
  entry: {
    app: './src/main.js',
    vendor: [
      'vue',
      'axio',
      'vue-router',
      'vuex',
      'element-ui',
      // 很长很长
    ],
  },
}
new webpack.optimize.CommonsChunkPlugin({
  name: 'vendor',
}),
```

> 寻找chunk依赖两次以上的模块，分开打包，移到vender中

* 处理

第二种方法

```javascript
entry: {
  // vendor: ['vue', 'axios'] // 删掉!
},

new webpack.optimize.CommonsChunkPlugin({
  name: 'vendor',
  minChunks: ({ resource }) => (
    resource &&
    resource.indexOf('node_modules') >= 0 &&
    resource.match(/\.js$/)
  ),
}),
```

>某些模块是来自 node_modules 目录的，并且名字是 .js 结尾的话，移到 vendor chunk 里去，如果 vendor chunk 不存在的话，就创建一个新的。

## 四、按需加载

require.ensure()依赖于promise，异步加载

```
require.ensure(dependencies: String[], callback: function(require), chunkName: String)

```

* 依赖：字符串数组，依赖的模块进行生命
* callback：加载所有模块之后的回调函数
* chunkname：提供给这个require的特定的chunkname；**通过提供 `require.ensure()`不同执行点相同的名称，我们可以保证所有的依赖都会一起放进相同的 文件束(bundle)。**

🌰

```
require('a');
require.ensure([], function(require){
    require('b');
});

\\ a.js
console.log('***** I AM a *****');

\\ b.js
console.log('***** I AM b *****');
```

entry和a打包在一个文件，b打包在一个文件



## 五、react懒加载实现

Loader:

```javascript
const config = require('config');
const root = config.get('root');
const path = require('path');

function parseComponent(chunk, path) {
  return `
  function(props) {
    return React.createElement(Lazy, {
      loader: function(cb) {
        require.ensure([], function() {
          cb(require("${path}"));
        }, "${chunk}");
      },
      subProps: props,
      chunk: "${chunk}",
    })
  }
  `;
}

function loader(content) {
  const routes = JSON.parse(content);
  const routeStr = [];
  for (let [key, path] of Object.entries(routes)) {
    routeStr.push(key, ":", parseComponent(key, path), ',');
  }
  let lazyLoadPath =
    path.relative(this.context,
      path.join(root, 'src', 'client', 'components', 'LazyLoad')
    ).replace(/\\/g, '/');
  if (!lazyLoadPath.match(/^\.\./)) {
    lazyLoadPath =  `./${lazyLoadPath}`;
  }
  return `
  var React = require('react');
  var Lazy = require("${lazyLoadPath}").default;
  module.exports = {
    default: {${routeStr.join('')}},
    __esModule: true,
  };
  `;
}

module.exports = loader;
```

```javascript
import React = require('react');
import { ref, unref } from '../../service/loader';
import { ContextTypes } from '../../utils/traceDependencies';
type Props = {
  loader: (cb: Function) => void,
  // tslint:disable-next-line:no-any
  subProps: any,
  chunk: string,
}

type State = {
  // tslint:disable-next-line:no-any
  Component: any;
}

/**
 * 异步加载组件
 */
export default class LazyLoad extends React.Component<Props, State> {
  static contextTypes = ContextTypes;

  didMount: boolean = false;

  constructor(props: Props) {
    super(props);
    this.state = {
      Component: null,
    };

    const { loader } = props;
    ref();
    // tslint:disable-next-line:no-any
    loader((c: any) => {
      if (c.default) c = c.default;
      if (this.didMount) {
        this.setState({
          Component: c
        })
      } else {
        this.state = {
          Component: c,
        };
      }
      unref();
    });
  }

  componentDidMount() {
    this.didMount = true;
  }

  render() {
    const { subProps, chunk } = this.props;
    if (this.context && this.context.dependences) {
      this.context.dependences[chunk] = true;
    }
    const { Component } = this.state;
    if (Component) {
      return <Component {...subProps}/>;
    } else {
      return null;
    }
  }
}
```

将xxx.async文件，会经由loader变成Lazy组件，每一个都用Lazy包装，

import进xxx.async文件时，创建组件实例，代码require.ensure()，webpack根据此来切分代码

> require import混用的时候，export default 会挂载在require().default上

两种情况，分别如何去确定分割后的代码依赖情况：

* 服务端渲染，根据context.dependecies去加载依赖的chunk
* router切换界面,  根据router的切换去异步加载相应的js文件，切换router就会执行webpack打包文件app中的代码，去require js文件