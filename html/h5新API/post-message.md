#传递消息
window.frames[0].postMessage(","http://lslib.com")（window.frames[]为**目标窗口**）
##参数
###要传递的消息 string
传递数组或者对象，应该使用JSON.stringfy()方法将其序列化
###目标窗口的源，包括协议，主机名以及URL（可选的）
可以传递一个完整的URL，但是除了协议，主机名和端口号之外的信息都会忽略。
* 安全特性：恶意代码或普通用户都可以在窗口中浏览新的未知文档，因此postmessage只传递给指定的窗口，不会传递给白喊非同源文档的窗口。
* 当然也可设为*通配符，如果要指定和当前窗口同源的话，也可以设为‘/’

#接收消息

	window.addEventListener('message',function(e){
	    if(e.source!=window.parent) return;
	    var color=container.style.backgroundColor;         
       			   window.parent.postMessage(color,'*');
	},false);

##当调用postMessage方法的时候，目标窗口的window对象就会触发一个message事件，并传递给一个属性对象
* data  第一个参数传递给postmessage方法的消息内容的副本
* source  消息源自的window对象
* origin	一个字符串，消息来源的URL