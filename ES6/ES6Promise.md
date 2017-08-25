>new Promise( function  (resolve,reject) {})

#基本用法
Promise对象代表的是未来某个将要发生的事件（异步操作），

**好处：可以将异步操作的流程表达出来，避免了层层嵌套的回调函数**

	var promise = new Promise(function(resolve, reject){
	    if(){
	        resolve(value);
	    }else {
	        reject(error);
	    }
	})
	
	promise.then(function(value){
	    //success
	}, function(error){
	    //failure
	})

promise 构造函数接收一个函数作为参数，该函数的两个参数分别是resolve和reject方法，resolve的方法将promise的状态变为成功；异步操作失败，reject方法pending到reject

**promise实例生成后，then可以指定resolve和reject方法**

#ajax实例

	function getJSON(url) {
	    var promise = new Promise(function(resolve, reject){
	
	        var xhr = new XMLHttpRequest();
	        xhr.open();
	        xhr.responseType = 'json';
	        xhr.setRequestHeader('Accept', 'application/json');
	        xhr.send();
	        xhr.onreadystatechange = handler;
	
	        function handler() {
	            if(this.status == 200 && this.readyState == 4) {
	                resolve(this.response);
	            } else {
	                reject(new error(this.statusText));
	            }
	        }
	    })
	    return promise;
	}
	
	getJSON('/posts.json').then(function(json){
	    console.log('contents:' + json);
	}, function(error) {
	    console.error('erroe',error);
	})

##.then方法返回的是新的promise对象：链式写法
then返回的是promise对象

	getJSON('/posts.json').then(function(){
		return json.post;
	}).then(function(post) {
		//proceed
	})

第一个函数回调完后，**将结果作为参数return**，传入第二个回调函数

回调函数**横向发展**

##catch方法：捕捉错误

是Promise.prototype.then(null, rejection)的别名，

	getJSON("/posts.json").then(function(posts) {
	  // some code
	}).catch(function(error) {
	  // 处理前一个回调函数运行时发生的错误
	  console.log('发生错误！', error);
	});

promise对象的错误具有冒泡性质，会一直向后递推，直到被捕获为止

##.all方法，.race方法
all（[p1,p2,p3]）将三个全部变成fulfilled，才会返回组成一个数组，传递给回调函数

race([p1,p2,p3])有一个fulfilled就执行
##.resolve方法，.reject方法将参数转为promise对象

如果promis的resolve方法的参数，不是具有then方法的对象，则返回一个新的promise，且状态为**fulfilled**

	var p = Promise.reject('出错了');
	// 等同于
	var p = new Promise((resolve, reject) => reject('出错了'))
	
	p.then(null, function (s) {
	  console.log(s)
	});
	// 出错了
**会立即执行回调函数**