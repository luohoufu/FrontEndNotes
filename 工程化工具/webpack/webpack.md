### resolve

用来配置文件路径的指向

定义文件的路径及后缀

节省webpack搜索文件的时间

优化引用模块的体验

* alias 资源路径重定向到另一个路径
* extensions 定义资源的默认后缀 引用资源时不需要写后缀



### resolveLoader

# webpack

- 模块打包机：分析项目结构，找到js模块以及一些其他的预设，打包问何时的格式共浏览器使用。

## 与gulp grunt相比

- gulp grunt 指明对某些文件进行类似编译组合压缩等任务的具体步骤，这个工具可以自动替你完成这些任务
- 把你的项目当做一个整体，通过一个给定的主文件，**从这个文件开始找到你的项目的所有依赖文件**，使用loaders处理它们，最好打包为一个浏览器可以识别的js文件

# 官方示例

 var webpack = require('webpack');
 var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');

 module.exports = {

```
 //插件项
 plugins: [commonsPlugin],
 //页面入口文件配置
 entry: {
     index : './src/js/page/index.js'
 },
 //入口文件输出配置
 output: {
     path: 'dist/js/page',
     filename: '[name].js'
 },
 module: {
     //加载器配置
     loaders: [
         { test: /\.css$/, loader: 'style-loader!css-loader' },
         { test: /\.js$/, loader: 'jsx-loader?harmony' },
         { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
         { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
     ]
 },
 //其它解决方案配置
 resolve: {
     root: 'E:/github/flux-example/src', //绝对路径
     extensions: ['', '.js', '.json', '.scss'],
     alias: {
         AppStore : 'js/stores/AppStores.js',
         ActionType : 'js/actions/ActionType.js',
         AppAction : 'js/actions/AppAction.js'
     }
 }
```

 };

## plugin 插件项

loaders是在打包构建过程中处理源文件的，一次处理一个，插件并不直接操作单个文件，直接对整个构建过程起作用

### 使用插件的方法

## entry 页面入口文件配置  output对应输出项配置（即入口文件最终要生成什么名字的文件、存在哪里）

```
module.exports = {
    entry: {
        main: './src/script/main.js',//打包成一个文件
        a: ['./src/script/a.js','./src/script/main.js'] //会打包成一个文件
    },
    output: {
        path: __dirname + '/dist/js',
        filename: '[name]-[hash].js'
    }
}
```

上述代码会打包两个文件生成page1.bundle.js,page2.bundle.js 存放到文件夹下

## module.loaders告知webpack每种类型的文件需要使用什么加载器来处理

```
module: {
        //加载器配置
        loaders: [
            //.css 文件使用 style-loader 和 css-loader 来处理
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            //.js 文件使用 jsx-loader 来编译处理
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
    }
```

- 上述-loader其实可以上略不写，多个loaders使用！链接
- 所有加载器需要npm来加载
- npm install url-loader --save-dev

## resolve

```
resolve: {
        //查找module的话从这里开始查找
        root: 'E:/github/flux-example/src', //绝对路径
        //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
        extensions: ['', '.js', '.json', '.scss'],
        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
        alias: {
            AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
```

# 运行webpack

## webpack

### webpack --display-error-details

### webpack --config XXX.js 使用另一份配置文件()来打包

### webpack --watch 监听变动并自动打包 -w

### webpack -p  压缩混淆脚本

### webpack -d  生成map映射条件，告知那些模块被最终打包到哪里



# 学习

# 1.style-loader和css-loader

```
import world from './world.js'
import style from 'style-loader!css-loader!./style.css'
```

使用方法如上

css-loader处理webpack文件；style-loader将处理好的文件创建style标签插入到html中

# 2.webpack的命令行

## webpack hello.js hello.bundle.js --module-bind 'css=style-loader!css-loader'

绑定模块处理loader

## --watch 自动打包

## --progress  查看进度

## --display-modules 列出模块每一步经过的处理

## --display-reasons  列出加载模块的原因

## --config 指定配置文件

# webpack的配置文件

## 小技巧

在npm package.json 中定义命令行的scripts

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "webpack": "webpack --config webpack.config.js --progress --dispaly-modules --colors --display-reason"
  },
```

并在npm 中命令行输入 npm run webpack即可

## html-webpack-plugin

在html中引用bundle.js来引入，但是随着输出名字的不确定，无法这样使用，因此使用plugin插件来解决

### 基本应用

```
  plugins: [
        new htmlWebpackPlugin({
            filename: 'index-[hash].html',
            //相对于上下文环境，会依据output的path生成到路径中，所以注意output的path
            template: 'index.html',//指定依据的模板文件
            inject: false,  //放在哪个标签中 'head'
            title:'webpack hah',
            date: new Date()
        })
    ]
 
```

主要有两个属性，具体属性参数可参考[此处](https://www.npmjs.com/package/html-webpack-plugin)

- htmlWebpackPlugin.files  主要包括一些文件参数
- htmlWebpackPlugin.options  主要包括有一些配置参数

### 想分别把script文件放在head和body底部 配置

```
模板文件index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="<%= htmlWebpackPlugin.files.chunks.main.entry%>"></script>
    指入口文件main生成的js文件
</head>
<body>
下面是ejx的模板文件语法，<% js语句 %><%= 取值 %>
<% for(var item in htmlWebpackPlugin.options) {%>
    <%= item %> : <%= JSON.stringify(htmlWebpackPlugin.options.item)%>
<% } %>

<script src="<%= htmlWebpackPlugin.files.chunks.a.entry%>"></script>   指入口文件a生成的js文件
</body>
</html>
```

### 上线的







