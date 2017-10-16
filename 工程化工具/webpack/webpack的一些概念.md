[TOC]

## output

### path

指定编译打包之后的文件存放位置

### publicPath

生产模式下，内嵌css img js文件的里的url值

例如在本地开发模式下，引用图片的url可能是‘./test.png’

但是在prod模式下 可能需要将文件定位到CDN上，因此需要更新文件的路径为在CDN上的路径

可以使用此项来让webpack生产环境下编译时自动更新这些

## chunk&&module

**每个资源都是一个module，几个module被打包为一个chunk**

