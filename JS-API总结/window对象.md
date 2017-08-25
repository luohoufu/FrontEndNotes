[TOC]



##一、navigator

###appCodeName 浏览器的代码名
###appName 浏览器名称
###appVersion 浏览器的平台和版本信息
###cookieEnabled 浏览器中是否启用cookie的布尔值
###platform 运行浏览器的操作系统平台
###userAgent 由客户机发送服务器的user-agent头部的值

```

function detectBrowser()

{

var browser=navigator.appName

var b_version=navigator.appVersion

var version=parseFloat(b_version)

if ((browser=="Netscape"||browser=="Microsoft Internet Explorer") && (version>=4))

  {alert("您的浏览器已经很棒了！")}

else

  {alert("您的浏览器需要升级了！")}

}
```

二、screen
###availHeight 显示屏幕的可用高度（除任务栏）
###availWidth 显示屏幕的额可用宽度
###height 返回屏幕的像素高度
###width 返回
###colorDepth 屏幕颜色位数
##三、historyof
###back() 返回前一个URL
###forward() 下一个URL
###go(num|URL) 返回某个具体页面 (-1代表back)
window.history.forward()
window.history.back()
##四、location
###hash 从#开始的url
###host 主机名和端口号
###hostname 主机名
###href 完整URL
###pathname 返回URL路径部分
###port 端口号
###protocol 协议
###search 从？号开始的URL
###assign(URL)  加载新文档 跳转到指定的url，当前页面会转为新页面内容，可以后退返回
###reload()  重新加载当前页面
###replace(URL)  新的文档代替当前文档  替换当前窗口页面，共用一个窗口，没有后退键
##五、document
###anchors[]
###images[]
###links[]
###forms[]
###cookie 与当前文档有关的cookie
###domain 当前文档的域名
###referrer 
###title
###URL
###open()
###close()
###write()
###writeIn()
```
打开一个流，收集document.write()方法的输出，再关闭输出流，并显示选定的数据

function createNewDoc()

  {

  var newDoc=document.open("text/html","replace");

  var txt="<html><body>学习 DOM 非常有趣！</body></html>";

  newDoc.write(txt);

  newDoc.close();

  }

```

##六、窗口控制
###moveBY
###moveTo
###resizeBy
###resizeTo
###scrollBy
###scrollTo
##七、焦点控制
###focus
###blur

##八、打开关闭窗口
###open('URL','窗口名称'，'窗口风格')
#####窗口风格
* height
* width
* left
* top
* location
* menubar
* resizeable
* scrollbars
* status
* toolbar

###close()关闭浏览器窗口

##九、定时器
###setTimeout
###clearTimeout
###setInterval
###clearInterval

##十、对话框
###alert()
###confirm('')显示确认框，确定显示true，取消返回false
###prompt（'','缺省文本'）返回用户输入，空返回null

##属性
###状态栏
###窗口位置
###其他