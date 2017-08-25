#一、基本


1. 每个函数内部都能访问一个特别变量Arguments，维护值所有传递到这个函数中的参数列表
2. **由于arguments被定义为函数内的一个变量，因此var arguments或者将arguments声明为形式参数，会导致原生arguments不会被创建**

#二、转化为数组
1. 类数组对象，不能直接使用数组方法
2. Array.prototype.sllice.call(Arguments)**转化比较慢，不推荐**

#三、传递参数
1. 将参数从一个函数传递到另一个函数
	<pre>
	function foo() {
		bar.apply(null,arguments);
	}
	function bar() {
	}
	</pre>

2. 创建解绑定包装器

	<pre>
	function Foo() {}
	
	Foo.prototype.method = function (a,b,c) {
		console.log(this,a,b,c);
	}

	//创建一个解绑定‘method’
	//输入参数参数：this，arg1，....
	Foo.method = function() {
		Function.call.apply(Foo.prototype.method,arguments)
	}

	我们想让Foo.method跟Foo.prototype.method完成一样的功能，但是想显式地指定方法体中的this对象（通过第一个参数），而不是Foo.prototype.method本身绑定的。


	//下面这个功能相同
	Foo.method = function() {
		var args = Array.prototype.slice.call(arguments);
		Foo.prototype.method.apply(args[0],args.slice(1))
	}
	</pre>