##一、typeof
	Value               function   typeof
	-------------------------------------
	"foo"               String     string
	new String("foo")   String     object
	1.2                 Number     number
	new Number(1.2)     Number     object
	true                Boolean    boolean
	new Boolean(true)   Boolean    object
	new Date()          Date       object
	new Error()         Error      object
	[1,2,3]             Array      object
	new Array(1, 2, 3)  Array      object
	new Function("")    Function   object
	/abc/g              RegExp     object
	function                       function
	new RegExp("meow")  RegExp     object
	{}                  Object     object
	new Object()        Object     object 
	null                           object

* 所有new出来的对象的typeof都是object， **除了function**
* 对于null有bug

注意基本类型的内置对象

**只能用于基本数据类型检测**和 **function**


##二、instanceof 用来检测原型链是否包含某个构造函数的prototype属性
* 通过**原型链来**检测类型
* 对**基本类型不起作用**，基本类型没有原型链
* 适用于**检测对象**

   3 instanceof Number // false
   	true instanceof Boolean // false
   	'abc' instanceof String // false
   	null instanceof XXX  // always false
   	undefined instanceof XXX  // always false
##三、constructor 属性返回一个指向创建了该对象原型的函数引用
* 很容易伪造
* constructoe只能检测对象，对基本数据类型无能为力，constructor是对象属性，在基本数据类型上会抛出typeError
* 但是在访问其他基本类型时，js会自动调动其构造函数来生成一个对象

   (3).constructor === Number // true
   	true.constructor === Boolean // true
   	'abc'.constructor === String // true
   	// 相当于
   	(new Number(3)).constructor === Number
   	(new Boolean(true)).constructor === Boolean
   	(new String('abc')).constructor === String

##三、toString
* toString是**最可靠的类型检测手段**，将当前对象转换为字符串并输出
* Array, Date等对象会重写从Object.prototype继承来的toString， 所以最好用**Object.prototype.toString**来检测类型。比如 **数字的toString方法就会把数组转化成string输出**

   toString = Object.prototype.toString;
   ​	
   	toString.call(new Date);    // [object Date]
   	toString.call(new String);  // [object String]
   	toString.call(Math);        // [object Math]
   	toString.call(3);           // [object Number]
   	toString.call([]);          // [object Array]
   	toString.call({});          // [object Object]
   	
   	// Since JavaScript 1.8.5
   	toString.call(undefined);   // [object Undefined]
   	toString.call(null);        // [object Null]

* toString也不是完美的，**无法检测用户自定义类型**，只能检测**scma标准中的那些内置类型**

   toString.call(new Animal)   // [object Object]
##四、跨窗口问题

		var iframe = document.createElement('iframe');
		var iWindow = iframe.contentWindow;
		document.body.appendChild(iframe);
		
		iWindow.Array === Array         // false
		// 相当于
		iWindow.Array === window.Array  // false


**总之，如果你要判断的是基本数据类型或JavaScript内置对象，使用toString； 如果要判断的时自定义类型，请使用instanceof。**