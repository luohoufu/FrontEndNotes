#基础
## 鼠标

* onclick
* onblclick



* **mouseenter**      只有在鼠标指针穿过**被选**元素时，才会触发mouseenter事件，**不支持冒泡，在子元素上不触发**


* **onmouseleave**     **不支持冒泡，在子元素上不触发**



* **mouseover **  无论鼠标是否穿过被选元素，都会触发mouseover事件，**子元素也触发**


* **onmouseout**     **子元素也触发**

  ​


* onmousedown
* onmouseup
* onmousemove
* onscroll

## 拖拽

### draggable设置为true，拖拽元素

* ondragstart 拖拽元素开始被拖拽的时候触发的时间，作用在拖拽元素上


* ondragend 拖拽元素结束拖拽触发



## 拖拽目标

* datatransfer


* ondragenter 拖拽元素进入目标元素
* ondragleave  拖拽元素离开目标元素
* ondragover 拖拽元素在目标元素上 一定要**执行preventDefault**，否则ondrop事件不会触发，
* ondrop
* onmousewheel

## 键盘

## 触摸

* touchstart
* touchmove
* touchend



* touches 当前屏幕上的所有手指的列表
* targetTouches 当前DOM元素上的手指的列表
* changedTouches涉及当前事件手指的列表