#一、getElementByClassName和querySelector及文档元素的data-set属性
#二、classlist属性
#三、xmlhttpRequest Level2、跨域http请求，服务器端发送事件标准
#四、web存储api和用于离线web应用的应用缓存
#五、audio,video
#六、canvas
#七、svg
#八、地理位置
允许js向浏览器询问用户真实的地理位置，会提前询问用户是否允许
* navigator.geolocation.getCurrentPosition() 获取用户当前位置
* navigator.geolocation.watchPosition（） 获取当前位置，同时不断监视当前位置
* navigator.geolocation.clearWatch() 停止监视用户位置

#九、历史记录管理
#十、跨域消息传递

# 十一、拖拽释放API

## 三个事件

- dragenter
- dragover
- drop

## 获取传过来的文件

- dragEvent.dataTransfer.files 是一个list array

## file的属性

- lastModefied 时间戳
- lastModifiedDate date对象
- name 文件名称
- size 大小
- type 文件类型MIME

## 上传进度

xhr.upload

# 二、在浏览器中拖拽

## mousedown绑在要移动的元素上

## mousemove绑在document上

## mouseup也绑在document上

## 另外要注意计算鼠标在拖动元素上的偏移位移