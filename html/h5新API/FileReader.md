#一、FileReader
允许用户异步读取存储在用户计算机上的文件的内容，使用file或blob对象指定要读取的文件或数据。

其中file对象可以是来自用户在input上传文件后返回的filelist对象，也可以是来自拖放操作生成的datatransfer对象，还可以是来自一个htmlcanvaselement上执行的mozgetasfile方法后返回的结果

##1.构造函数
FileReader()
##2.状态常量
* empty 0 还没有加载任何数据
* loading 1 数据正在被加载
* done 2 已完成全部的读取请求

##3.属性
* error 读取文件时发生的错误
* readyState 当前状态
* result 读取到的文件内容，只在读取完成后有效，数据的格式取决于方法
##3.方法概述
###abort
###readAsArrayBuffer()
* 读取的对象以ArrayBuffer对象表示
###readAsBinaryString()
##4.readAsText
###readAsDataURL()
* 读取后的result属性为URL格式的字符串以表示所读取的内容
* 可以实现图片的本地预览功能（设置img.src=result即可）

##5.事件处理程序
* onabort 读取操作被终止是调用
* onerror 读取操作发生错误
* onload 读取操作完成时调用
* onloadend 读取操作完成时调用，不管成功失败，在onload，onerror之后调用
* onloadstart 读取操作开始之前
* onprogress 读取中  **e.loaded**
#ArrayBuffer
##概述
通用的，固定长度的二进制数据缓冲区，不能直接操纵ArrayBuffer的内容，应该创建一个表示特定格式的视图（typedArray和dataview视图）来读写，视图的作用是制定格式解读**二进制数据**