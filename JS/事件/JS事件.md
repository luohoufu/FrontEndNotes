[TOC]



#一、js三种事件处理程序

##1 内联模型 html事件处理程序
和html混写，没有和html分离
##2 脚本模型 DOM0级事件处理程序
在js中处理事件，elem.on+事件名称
##3 DOM2级事件处理程序
addEventListener('click',callback,false)

**第三个参数：当第三个参数设置为true就在捕获过程中执行，反之就在冒泡过程中执行处理函数。**

##4 IE事件处理程序
attachEvent('onclick',callback) detachEvent()


#二、各类事件
##鼠标事件
* onclick 点击或者按下回车键时触发
* ondbclick 双击鼠标
* onmousedown 按下鼠标还未弹起
* onmouseup 用户释放鼠标按钮
* onmouseover 鼠标移到某个元素上方
* onmouseout 鼠标移出
* onmousemove 鼠标指针在元素上移动
##键盘事件
* onkeydown
* onkeypress
* onkeyup
##html事件
* onload：页面完全加载后在window上面触发，
* onselect：当用户选择文本框中的一个或多个字符触发
* onchange：
* onfocus：
* onblur：
* onsubmit：
* onreset：
* onresize：
* onscroll：
##touch事件
* touchstart
* touchmove
* touchend

####触摸列表
* touches：当前屏幕所有触摸点的列表
* targettouches：当前对象上所有的触摸点列表
* changedtouches：涉及当前事件的触摸点的列表


#三、事件的三个阶段
##1 事件捕获（低ie不支持）
事件捕获从最外层也就是根节点开始开始向内传播，直到发生点击事件的最小元素
<br>
addEventListener
<pre>
element.addEventListener(event, function, useCapture)
</pre>

第三个参数 表示在事件捕获阶段调用处理函数
##2 目标对象的事件处理程序
##3 冒泡阶段
* 时间从内层向外层冒泡传播事件
  停止冒泡 e.stopPropagation（）；

  stopImmediatePropagation();如果某个元素有多个**相同类型事件**的事件监听函数,则当该类型的事件触发时,多个事件监听函数将按照顺序依次执行.如果某个监听函数执行了 `event.stopImmediatePropagation()方法`,则除了该事件的冒泡行为被阻止之外(`event``.stopPropagation方法的作用`),该元素绑定的**后序相同类型事件的监听函数的执行也将被阻止.**

![demo](http://images.cnitblog.com/blog/477973/201302/18141423-8bd09a9c1e184df9a13b6e26b88348f3.jpg)
##4 例子

<pre>
var parent = document.getElementById("parent");
var child = document.getElementById("child"); 
document.body.addEventListener("click",function(e){ console.log("click-body"); },false);
parent.addEventListener("click",function(e){ console.log("click-parent---事件传播"); },false);

parent.addEventListener("click",function(e){ console.log("click-parent---事件捕获"); },true);
child.addEventListener("click",function(e){ console.log("click-child"); },false);
</pre>

click-parent---事件捕获
click-child
click-parent---事件传播（开始冒泡）
click-body
#二、ie和w3c不同绑定事件的区别，参数是什么，对象e参数区别
##1 event常用属性
* altkey alt是否被按下 ctrlkey metakey
* button 哪个鼠标按键
* clientx   可视区域水平坐标
* screenx   屏幕区
* keyCode 
* type 事件名称
* **target 目标节点（ie srcelement）**  事件的调用对象
* currentTarget  事件的处理对象一般指 **父级**
* preventDefault 不执行事件关联的默认动作 ie event.returnValue=false;
* stopPropagation（）停止传播派发事件 ie不支持
* nodeName  找到当前元素的标签名

##2 ie特色
* cancelBubble  设为true
* srcelement 事件对象
* returnvalue = true
* fromElement toElement
* offsetx 当前元素的位置

##3 绑定事件区别
* addEventListener('click',function(){},false)
* attachEvent（'onclick', function(){}）

 兼容事件处理
 <pre>
 var addEvent = {
 	on:function(elem, type, handler){
 		if(elem.addEventListener{
 			elem.addEventListener(type, handler, false);
 		}else if(elem.attachEvent){
 			elem.attachEvent('on'+type, handler);
 		}
 	}
 }

 var elem = document.getElementById('img');
 addEvent.on(elem, 'click', function(){
 	console.log('添加点击事件')
 })
 </pre>

##4 事件对象定位
* event
* ie8之前window.event

##5 鼠标当前坐标 
* ie event.x event.y 相对于最近的父元素的位置
* ff event.pagex event.pagey  网页
* 通用 event.clientx event.clienty

##6 鼠标当前坐标(当前对象)
* ie event.offsetx event.offsety
* ff event.layerx event.layery

##7 获取目标
* IE：oEvent.srcElement
* DOM : oEvent.target

##8 阻止事件默认行为
* IE：oEvent.returnValue = false
* DOM : oEvent.preventDefault

##9 组织事件冒泡
* IE：oEvent.cancelBubble = true;
* DOM:oEvent.stopPropagation();
#三、事件代理原理及优缺点
##1 原理
将事件到父级元素 满足一条匹配条件时，才会被执行，采用事件冒泡机制实现
##2 优点
* **大量减少内存占用**，减少时间注册，例table上代理所有td的click事件
* 新增对象无需再次绑定 对动态内容尤为合适

##3 原生js实现事件代理，兼容浏览器
```javascript
function delegateEvent(interfaceEle, selector, type, fn) {
if(interfaceEle.addEventListener()) {
	interfaceEle.addEventListener(type, eventFn);
}
else {
	interfaceEle.attachEvent(type, eventFn);
}

function eventFn(fn) {
	var e = event || window.event;
	var target = e.target || e.srcElement;
	if (matchSelector(target, selector)) {
		if(fn) {
			fn.call(target, e); //改变this指向
		}
	}
}

function matchSelector(ele, selector)  {
	
	//if use id
	if(selector.charAt(0) === '#') {
		return ele.id === selector.slice(1);
	}

	//if use class
	if(selector.charAt(0) === '.') {
		return ele.className === selector.slice(1);
	}

	return ele.tagName.toLowerCase() === selector.toLowerCase();
}
}

//调用

var odiv = document.getElementById('odiv');

delegateEvent(odiv,'a','click',function () {
// body...
alert('1');
})

```
#四、实现事件模型

	function Emitter() {
	this._listener = {};     //listener
	}
	
	//注册事件
	
	Emitter.prototype.bind = function (eventName, callback) {
		var listener = this._listener[eventName] || [];
		listener.push(callback);
		this._listener[eventName] = listener;
	}
	
	Emitter.prototype.trigger = function (eventName) {
		var args = Array.prototype.slice.apply(arguments).slice;//将arguments转换为数组，并获取参数
		var listener = _listener[eventName];
	
		listener.forEach(function(item) {
			try {
				item.apply(this,args);     //this调用此函数
			}catch(e) {
				console.error(e);
			}
		})
	}
	
	var emmitter = new emmitter();
	emmitter.bind('myevent',function(arg1){
		console.log(arg1,arg2);
	})
	emmitter.trigger('myevent','','')
#五、lazyman
<pre>
//lazyman实现 使用数组的形式将函数存入，然后从头执行，依次向后

function LazyMan(name) {
	this.task = [];  
	var self = this;
	var fn = (
		function(n){
			var name = n;
			return function(){
				console.log('Hi! this is '+name+'!');
				self.next();
			}
		})(name);
	this.task.push(fn);
	setTimeout(function(){
		self.next();
	},0);	
}

LazyMan.prototype.next = function(){
	var fn = this.task.shift();
	fn&&fn();
}

LazyMan.prototype.eat = function(){
	var self = this;  //下面函数不能直接使用this
	var fn = function(name){
		return function(){
			console.log('eat' + name);
			self.next();
		}
	}
	this.task.push(fn);
	return this;//实现链式低啊用函数
}

LazyMan.prototype.sleep = function(){
	var self = this;
	var fn = function(time){
		return function(){
			setTimeout(function(){
				console.log('wake up after' + time);
			self.next();
		},time*1000)
		}
	}
	this.task.push(fn);
	return this;
}

LazyMan.prototype.sleepfirst = function(){
	var self = this;
	var fn = function(time){
		return function(){
			setTimeout(function(){
				console.log('wake up after' + time);
			self.next();
		},time*1000)
		}
	}
	this.task.unshift(fn);
	return this;
}
</pre>



