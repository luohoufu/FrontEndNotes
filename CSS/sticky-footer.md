![](https://cdn.css-tricks.com/wp-content/uploads/2016/05/sticky-footer-1.svg)

## 设置负边距

原理：给定一个content-wrapper表示内容部分，footer表示页脚部分，给content-warpper设置min-height为100%，且content-wrapper中放push和footer同高占位，使得内容不够时，也可撑满界面，不要忘记给html,body设置height100%，不然min-height不起作用

## 将页脚的顶部外边距设为负数

原理：给content-wrapper设置min-height，设置padding-bottom，并且给footer设置margin-top：-

## 使用calc设置内容高度

原理：content-wrapper设置为min-height：calc（100vh - 70px）

## 使用flex弹性布局

## 使用grid