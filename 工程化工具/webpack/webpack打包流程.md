[TOC]

###### ![](https://img.alicdn.com/tps/TB1GVGFNXXXXXaTapXXXXXXXXXX-4436-4244.jpg?_=5852987)

## 一、shell与config解析

命令行输入webpack 后，操作系统 运行nodemoudles下面的webpack的shell脚本文件，基本文件调用webpack中的主js文件

将webpack.config.js和node命令行的参数配置合并为option传入下一个流程中

```javascript
var webpack = require("../lib/webpack.js");
var compiler = webpack(options);
```

### 1.optimist

实现对命令行的解析，并保存键值对optimist.argv

### 2.config合并，插件加载

optimist.argv被传入covert.argv.js中，转换为options，与config中的options合并，并加载相应插件

## 二、编译与构建

### 1.核心对象compilation

![](https://img.alicdn.com/tps/TB1UgS4NXXXXXXZXVXXXXXXXXXX-693-940.png?_=5852987)

### 2.编译与构建主流程

* make
* 调用addEntry方法，调用私有方法 `_addModuleChain`

#### _addModuleChain

```javascript
Compilation.prototype._addModuleChain = function process(context, dependency, onModule, callback) {
   var start = this.profile && +new Date();
   ...
     // 根据模块的类型获取对应的模块工厂并创建模块
   var moduleFactory = this.dependencyFactories.get(dependency.constructor);
   ...
   moduleFactory.create(context, dependency, function(err, module) {
       var result = this.addModule(module);
           ...
       this.buildModule(module, function(err) {
       ...
         // 构建模块，添加依赖模块
       }.bind(this));
   }.bind(this));
 };
```

* 获取模块并创建模块，并将模块保存到compilation对象上

* 构建模块 doBuild() 

  * 使用对应模块的loader进行加工，最终生成一个js文件 module
  * 使用acorn解析源文件生成AST语法树

  ```javascript
  Parser.prototype.parse = function parse(source, initialState) {
     var ast;
     if(!ast) {
         // acorn以es6的语法进行解析
         ast = acorn.parse(source, {
             ranges: true,
             locations: true,
             ecmaVersion: 6,
             sourceType: "module"
         });
     }
       ...
   };
  ```

  * 遍历 AST，遇到require依赖时，创建一个依赖的模块数组，模块加入到此数组中，当前模块构建完成之后，依次构建数组中依赖的模块，异步对数组中的依赖module进行build，依次递归构建

```javascript
Compilation.prototype.addModuleDependencies = function(module, dependencies, bail, cacheGroup, recursive, callback) {
    // 根据依赖数组(dependencies)创建依赖模块对象
    var factories = [];
    for(var i = 0; i < dependencies.length; i++) {
        var factory = _this.dependencyFactories.get(dependencies[i][0].constructor);
        factories[i] = [factory, dependencies[i]];
    }
        ...
      // 与当前模块构建步骤相同
  }
```

![](https://img.alicdn.com/tps/TB1WOiRNXXXXXcJaXXXXXXXXXXX-445-1228.png?_=5852987)

### 3.构建细节

module 是 webpack 构建的核心实体，也是所有 module的 父类，它有几种不同子类：`NormalModule` , `MultiModule` ,`ContextModule` , `DelegatedModule` 

但是都会调用build进行构建

```javascript
// 初始化module信息，如context,id,chunks,dependencies等。
NormalModule.prototype.build = function build(options, compilation, resolver, fs, callback) {
    this.buildTimestamp = new Date().getTime(); // 构建计时
    this.built = true;
    return this.doBuild(options, compilation, resolver, fs, function(err) {
        // 指定模块引用，不经acorn解析
        if(options.module && options.module.noParse) {
            if(Array.isArray(options.module.noParse)) {
                if(options.module.noParse.some(function(regExp) {
                        return typeof regExp === "string" ?
                            this.request.indexOf(regExp) === 0 :
                            regExp.test(this.request);
                    }, this)) return callback();
            } else if(typeof options.module.noParse === "string" ?
                this.request.indexOf(options.module.noParse) === 0 :
                options.module.noParse.test(this.request)) {
                return callback();
            }
        }
        // 由acorn解析生成ast
        try {
            this.parser.parse(this._source.source(), {
                current: this,
                module: this,
                compilation: compilation,
                options: options
            });
        } catch(e) {
            var source = this._source.source();
            this._source = null;
            return callback(new ModuleParseError(this, source, e));
        }
        return callback();
    }.bind(this));
};
```

## 三、打包输出

所有模块及其依赖模块build完成之后，webpack监听seal事件，调用插件对build之后的源代码进行封装，逐次对每个module和chunk进行整理。生成编译后的源码，进行拆分，合并，生成hash

```javascript
Compilation.prototype.seal = function seal(callback) {
    this.applyPlugins("seal"); // 触发插件的seal事件
    this.preparedChunks.sort(function(a, b) {
        if(a.name < b.name) return -1;
        if(a.name > b.name) return 1;
        return 0;
    });
    this.preparedChunks.forEach(function(preparedChunk) {
        var module = preparedChunk.module;
        var chunk = this.addChunk(preparedChunk.name, module);
        chunk.initial = chunk.entry = true;
        // 整理每个Module和chunk，每个chunk对应一个输出文件。
        chunk.addModule(module);
        module.addChunk(chunk);
    }, this);
    this.applyPluginsAsync("optimize-tree", this.chunks, this.modules, function(err) {
        if(err) {
            return callback(err);
        }
        ... // 触发插件的事件
        this.createChunkAssets(); // 生成最终assets
        ... // 触发插件的事件
    }.bind(this));
};
```

### 1.生成最终的assets

封装的过程调用createChunkAssets，进行模块的代码封装

通过判断是入口js还是异步加载的js进行不同使用不同的Template进行render

模块进行封装，添加到sourcemap，对应assets，包含单个文件名和文件代码，保存在complition对象上保存为assets属性

![](https://img.alicdn.com/tps/TB1cz5.NXXXXXc7XpXXXXXXXXXX-959-807.png?_=5852987)

## loader

输入文件，输出文件，支持loader串型使用，例如：style!css!less!./foo.less

## plugin

loader只能处理单一文件的输入输出

## entry

 Dss

## 参考

[webpack原理](http://www.thkdog.com/html5/2015/05/08/webpack.html)