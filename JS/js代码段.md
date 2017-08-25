[TOC]

## 一、对象深拷贝

### 1.json.stringify

缺点：

* 会抛弃对象的constructor，都为Object
* 只能处理Number，string，boolean，array即那些能被直接表示的数据结构，regexp对象无法深拷贝
* function会被直接忽略

```
function deepClone(initalObj) {
    var obj = {};
    try {
        obj = JSON.parse(JSON.stringify(initalObj));
    }
    return obj;
}
```

### 2.递归拷贝

```
function deepCopy(obj, finalObj) {
	var obj = finalObj || {};
	for(var i in initalObj) {
		var prop = initalObj[i];
		//防止循环死循环
		if(prop === obj) {
		  continue;
		}
		if(typeof prop === 'Object') {
		  obj[i] = (prop.constructor === Array) ? []:{};
		  deepCopy(prop, obj[i]);
		}else {
          c[i] = p[i];
		}
	}
	return obj;
}
```

### 使用Object.create()

## $.fn.extend函数的实现

同上

## 二、function.prototype.bind

```
if(!Function.prototype.bind){
  Function.prototype.bind = function(oThis){
    var args = Array.prototype.slice.call(arguments, 1),
    	fToBind = this,
    	fNop = function () {},
    	fBound = function () {
          return fToBind.apply(this instanceof fNOP ？ this : oThis || this,aArgs.concat(Array.prototype.slice.call(arguments))),
    	}
    	fNOP.prototype = this.prototype;
    	fBound.prototype = new fNOP();
    	return fBound;
  }
}
```

## 三、设计基于观察者模式的事件绑定机制

## 四、正则表达式

### 1.在文章中发现敏感词，并表示出来

```
function highlight(words,node) {
	var regStr = "/(";
	for (index in words) {
		regStr += words[index] + '|';
	}
	regStr = regStr.substring(0, regStr.length - 1);
	regStr += ")/g";
	var html = node.innerHTML;
	html = html.replace(eval(regStr), function($0){
		return '<span>' + $0 + '</span>';
	});
	node.innerHTML = html;
}
function main() {
	var words = ["你" ,"我", "的","了"];
	var text = document.getElementsByClassName('text')[0];
	highlight(words,text);
}
```

### 2.trim实现

```
function trim(str) {
	str = str.replace(/(^\s*)|(\s*$)/g , "");
	return str;
}
```

### 3.提取url中的值

```
function getParams(name) {
	var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)','i');
	var match = location.search.slice(1).match(reg);
	if(match){
		return match[1];
	}else{
		return;
	}
}
```

## 五、监听上传时间

```javascript
dragArea.addEventListener('dragenter',function(e){
	e.stopPropagation();
	e.preventDefault();
});
dragArea.addEventListener('dragover', function(e) {
	e.stopPropagation();
	e.preventDefault();
})

dragArea.addEventListener('drop', handleDrop)
function handleDrop(e) {
	e.stopPropagation();
	e.preventDefault();
	var files = e.dataTransfer.files;
	for(var i =0; i < files.length; i ++){
		var li = document.createElement('li');
		var progressbar = document.createElement('div');
		var name = document.createElement('span');
		progressbar.className = "progressBar"; 			
		name.innerHTML = files[i].name;  
		li.appendChild(name); 
		li.appendChild(progressbar); 
		fileList.appendChild(li); 
		uploadFile(files[i],progressbar);
     }	
}
function uploadFile(file, progressbar) {
	var xhr = new XMLHttpRequest();
	var upload = xhr.upload;
	upload.onprogress = function(e) {
		if(e.lengthComputable){
			console.log('yes');
			var percentage = Math.round((e.loaded)/e.total)
			progressbar.textContent = percentage*100 + '%';	
		}
	} 

	var fd = new FormData();
	fd.append('file',file);

	// var reader = new FileReader();
	// reader.onloadstart = function() {
	// }
	// reader.onprogress = function(e) {
	// }
	// reader.readAsBinaryString(file);

	xhr.open("POST", "/upload"); 
	xhr.send(fd); 
```

## 六、tab切换

```javascript
 	for(var i=0;i<btns.length;i++) {
 		btns[i].index = i;//重点
 		btns[i].onmousemove = function() {
 			for(var j=0;j<btns.length;j++) {
 				btns[j].className = "";
 			}
 			this.className = "current";
 			for(var i=0;i<divs.length;i++){
			 	divs[i].style.display = "none";
			}
			divs[this.index].style.display ="block";
 		}
 	}
```

## 七、轮播图

## 八、当点击一个页面内的元素时，alert 出这个元素的标签名

## 九、展平多维数组

```javascript
function flatten(arr) {
  var flattened_arr=[];
  for(var i=0;i<arr.length;i++){
    if(arr[i] instanceof Array){
      Array.prototype.push.apply(flattened_arr,arr[i])
    }else{
      flattened_arr.push(arr[i]);
    }
  };
  return flattened_arr;
}
```

## 十、一个字符串web(dev(ni(cat(new)))),找出第n个括号中的内容

```javascript
正则匹配。。。
function getText(str) {
  const arr = str.match(/\(\w*/g)
  console.log(arr);
}

getText('web(dev(ni(cat(new))))');
```

## 十一、深度优先遍历dom树DFS

```javascript
function interator(node) {
  console.log(node);
  if(node.children.length) {
    for(let i=0;i< node.children.length;i++) {
      interator(node.children[i])
    }
  }
}
```

## 十二、广度优先遍历dom树BFS

先进先出的策略，将节点放入数组中，一次放一层节点，取出一个节点，就在队列末尾将他的子节点放到队列中。

```javascript
function interator(node) {
  const arr = [];
  arr.push(node);
  while(arr.length>0) {
    node = arr.shift();
    console.log(node);
    if(node.children.length) {
      for(var i = 0; i<node.children.length;i++) {
        arr.push(node.children[i]);
      }
    }
  }
}
```

## 十三、实现sum(1,2),sum(1)(2)

```

```

> 柯里化，部分和求值，传递给函数一部分参数来调用它，返回一个函数去处理剩下的参数

```
举个🌰
function sum(a,b) {
  return a + b;
}

function sumCurring(a) {
  return (b) => a + b
}

返回的函数通过闭包即住了第一个参数
```