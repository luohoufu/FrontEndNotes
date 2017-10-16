[设备像素，设备独立像素，css像素](http://yunkus.com/physical-pixel-device-independent-pixels/)

[设备像素比devicePixelRatio简单介绍](http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/)

[7种方法解决移动端retina屏幕1px边框的问题](http://www.jianshu.com/p/7e63f5a32636)



[TOC]



通常我们说的css的px其实指的是设备独立像素，逻辑像素，而不是实际的像素点，屏幕上的像素点成为物理像素，

在电脑端，其比例是1:1，但是移动端有可能是2:1，也就是两个物理像素点渲染一个px

此时640像素点的设计稿需要转换成物理像素点

## 一、几个定义

**设备像素：物理像素，设备能控制的显示的最小单位，可以把其看成显示器上一个个的点。像素点。**

css像素：独立与设备用于逻辑上衡量像素的单位，也就是我们平时做网页时用的css像素单位，是抽象的，不是实际存在的。

**设备独立像素：密度无关像素／逻辑像素，物理像素和设备独立像素之间存在一定的对应关系。**



由于pc端设备像素和设备独立像素相同，

移动端获取的是设备独立像素，而不是设备像素，因此**设备的分辨率对于web开发者来说是无法通过代码获得的，是完全透明的。**



设备像素比 = 设备像素（可以称之为光点）／设备独立像素

**对应于 window.devicePixelRatio设备上物理像素和设备独立像素（dips）的比例**

![](http://yunkus.com/wp-content/uploads/2016/07/physical-pixel-device-independent-pixels-1.png)

![](http://yunkus.com/wp-content/uploads/2016/07/physical-pixel-device-independent-pixels-2.png)

![](http://yunkus.com/wp-content/uploads/2016/07/physical-pixel-device-independent-pixels-3.png)

## 二、1px边框变粗问题的由来

1px元素显示变粗，不识别0.5px，所以变粗，

0.5px

解决方法：

### 1.伪类+transform

把原生元素的border去掉，利用：before或者after重做border，并transform scale缩小一半，原来的元素相对定位，新做的border绝对定位；

```css
.scale-1px{
  position: relative;
  margin-bottom: 20px;
  border:none;
}
.scale-1px:after{
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  border: 1px solid #000;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 200%;
  height: 200%;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
  -webkit-transform-origin: left top;
  transform-origin: left top;
}
```

### 2.viewport + rem方法

```
devicePixelRatio = 2
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

devicePixelRatio = 3
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">
```



## 三、移动端UI给640设计稿的宽度，自己要除2

因为给的是实际的物理像素，不是设备独立像素，所以要除2 除3 得到设备独立像素。也就是css像素，

设计师可以设计

icon.png          

icon@2x~iphone.png

icon@3x~ipad.png

不同格式可以加载不同资源，以免图片模糊