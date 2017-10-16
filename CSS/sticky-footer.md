[TOC]



![](https://cdn.css-tricks.com/wp-content/uploads/2016/05/sticky-footer-1.svg)

## 将页脚的顶部外边距设为负数

原理：给content-wrapper设置min-height，设置padding-bottom，并且给footer设置margin-top

## 使用calc设置内容高度

原理：content-wrapper设置为min-height：calc（100vh - 70px）

## 使用flex弹性布局

display： flex

flex-flow： column

min-height：100%；



flex：1

## 使用grid