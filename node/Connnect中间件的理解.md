#主要内容
1. 原生httpserver遇到的问题
2. 中间件思想
3. connect实现
4. express简介

#http
经典的httpserver代码

	var http = require('http');
	
	var server = http.createServer(requestHandler);
	function requestHandler(req.res){
		res.end('hello visitor');
	}
	
	server.listen(3000);

##例如请求过来的业务逻辑上如下
* 检测请求中请求体是否存在，存在则解析请求体
* 查看请求体中的id是否存在，若存在则去数据库查询
* 根据数据库结果返回约定的值

一般来说，我们的想法是抽离函数，每个函数一个逻辑，之后使用回调函数调用；如下
	
		function parseBody(req, callback){
			//从req解析body
			callback(body);
		}
		
		function checkId(body, callback){
			// 根据body.id在database中检测，返回结果
			callback(dbresult);
		}
		function returnResult(dbresult, res){
			if (dbResult && dbResult.length > 0) {
		    	res.end('true');
		  	} else {
		    	res.end('false');
		  	}
		}
		
		function requestHandle(req,res) {
			parseBody(req, function(body){
				checkId(body, function(dbresult){
					returnResult(dbresult,res);
				})
			});
		}

**但是如果有30个逻辑呢，那就会出现30个});以及30个err异常，根本看不清自己写的逻辑在30层的哪一层，极容易出现多次返回或返回地方不对等的情况** 也就是**回调金字塔**

##解决方法
* eventProxy 事件发布订阅模式
* blueBird promise
* async 异步流程控制库
* generator es原生genetator

##connect异步流程控制的思想

###异步流程控制库的思想
* 传入待执行的函数列表，记为funlist，流程控制库的任务是让这些函数**顺序执行**
* funlist函数每当调用callback就会执行下一个list里的函数

		var middlewares = [
			function fun1(req, res, next){
				parseBody(req, function(err, body) {
		
					if(err) return next(err);
		
					req.body = body;
					next();
				})
			},
		
			function fun2(req, res, next){
				checkId(req.body.id, function(err, rows){
		
					if(err) return next(err);
		
					res.dbResult = rows;
					next();
				})
			},
		
			function fun3(req, res, next){
				if (res.dbResult && res.dbResult.length > 0) {
		      		res.end('true');
		    	}
		    	else {
		      		res.end('false');
		    	}
		    	next();
			}
		]
		
		function requestHandler(req, res) {
			var i = 0;
			function next(err) {
				if (err) {
					return res.end('error:', err.toString());
				}
		
				if(i < middlewares.length) {
					middlewares[i++](req, res, next);
				}else {
					return;
				}
			}
			next();
		}

###整体思路
上面的middlewares+next完成了业务逻辑的链式调用，而middlewares里的每个函数，每个都是一个中间件
* 将所有处理逻辑函数（中间件）存储在一个list中
* 请求到达时循环调用list中的处理逻辑函数（中间件）；

#connect源码阅读
##createServer
