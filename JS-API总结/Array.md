[TOC]



##易错点

	for in中的 item 是 index
	for(var item in a){a[item] = a[item]+1;console.log(item)};
	0
	1
	2

有callback函数的，只有带上array才能修改数组
#一、基础
##1 删除数组
delete 数组名[下标]
##2 遍历数组
for(var 数组元素索引 in 数组)
for(var 数组元素 of 数组)
##3 数组属性
* constructor 引用数组对象的构造函数
* length 数组长度


#二、es3 方法
##1. 添加和删除
**以下方法类对象使用时，若length不存在，添加length属性 值为0（会修改索引0处的值）**
### push（添加元素到数组末尾  返回新的length属性值）
1.  arr.push(ele,ele...) 
2.  Array.prototype.push.apply(a, [3,4,6,1]); 末尾添加第二个数组
3.  类数组对象（除String，字符串为基本类型不可改变）可通过apply/call使用；

### pop （删除数组末尾元素   返回删除元素）
1. arr.pop() 返回删除元素，数组为空 返回undefined

### unshift（添加元素到数组首部   新的length属性值）
1. arr.unshift(ele,ele...) 返回新的length属性值

###shift（删除数组首部元素   返回删除元素）
1. arr.shift() 返回删除元素，数组为空 返回undefined

### splice （删除数组中的一个区间的元素，并在此处添加元素，返回删除的元素）
1. arr.splice(start删除开始位置[可为负值], deleteCount删除数目, item1,item2,...) 返回删除的元素组成的数组，没有删除返回空数组
2. var removed = myFish.splice(0, 2, "parrot", "anemone", "blue");
3. **删除数目设为0**，则可直接在**start**处开始插入元素，start之后的元素包括start处的元素后移

### concat （合并两个数组  返回新数组）
1. var new_array = old_array.concat()数组*或非数组值
2. concat不修改调用他的对象和参数中数组的值，直接拷贝到新数组中。**（对象引用：浅拷贝；字符串和数字：原始值）**
3. 类数组可使用，但结果不同.返回的对象是数组，而不是对象。 **无length属性也可使用，且不会添加length属性 **

 <pre>
 var o = {0:"a", 1:"b", 2:"c",length:3};
 var o2 = Array.prototype.concat.call(o,'d',{3:'e',4:'f',length:2},['g','h','i']);
 console.log(o2);//[{0:"a", 1:"b", 2:"c", length:3}, 'd', {3:'e', 4:'f', length:2}, 'g', 'h', 'i']
 </pre>
4. alpha.concat(1, [2, 3]);//1,2,3
##2.子数组
### slice（返回提取的元素）
1. arr.slice(begin,end(不包括此索引))； [begin,end)半开半闭区间
2. slice不修改调用他的对象，直接拷贝到新数组中。**（对象引用：浅拷贝；字符串和数字：原始值）**
3. 若取不到值 **返回空数组 **
4. 类数组无length属性不能使用，返回空数组

##3. 数组排序
**类数组对象使用下面方法时，若length属性小于2或者不为数值或不存在时，类数组对象无变化；length不存在也不会去创建length属性。**
### reverse （颠倒数组，返回数组） 
1. arr.reverse()
### sort （数组排序，返回数组）
1. arr.sort(compareFunction),省略compareFunction，元素按照转换为**数组的unicode位点**排序（**9在80之前**）
2. comparefuntion:按照返回值排序（**返回-1,1,0数值，返回true/false不同浏览器会有不同排序结果**）
   <pre>
    function compare(a, b) {

   if (a is less than b by some ordering criterion) {
    //a排在b前面，返回小于0
    return -1;
   }

   if (a is greater than b by the ordering criterion) {
    //b排在a前面，返回大于0
    return 1;
   }
   // a must be equal to b
   return 0;
    }
    </pre>
3. 数组元素为非ASC字符的字符串（包含中文字符等非英文字符的字符串）需使用String.localeCompare()

 <pre>
 var array = ['互','联','网','改','变','世','界'];
 var array2 = array.sort();

 var array = ['互','联','网','改','变','世','界'];//重新赋值,避免干扰array2
 var array3 = array.sort(function (a, b) {
   return a.localeCompare(b);
 });

 console.log(array2);//["世", "互", "变", "改", "界", "网", "联"]
 console.log(array3);//["变", "改", "互", "界", "联", "世", "网"]
 </pre>

##4. 数组转换


**元素为undefined或null则转为空字符串,不会跳过**

### toString （转为字符串返回）
1. arr.toString()
2. array对象覆盖了Object的toString对象。由数组每个元素各自的toString()**[eg:Object: Object.prototype.toString()
   Number: Number.prototype.toString()
   Date: Date.prototype.toString()]**返回值,再调用join方法（逗号隔开）链接组成，
3. 当数组被当做文本值或者进行字符串链接操作时，将自动调用toString方法。

 <pre>
 var monthNames = ['Jan', 'Feb', 'Mar', 'Apr'];
 var myVar = monthNames.toString(); // assigns "Jan,Feb,Mar,Apr" to myVar.

 monthNames += 'May';//Jan,Feb,Mar,AprMay
 </pre>

### toLocaleString
1. arr.toLocaleString();
2. 由数组每个元素各自的toString()**[eg:Object: Object.prototype.toLocaleString()
   Number: Number.prototype.toLocaleString()
   Date: Date.prototype.toLocaleString()]**返回值,再调用join方法（逗号隔开）链接组成，
   <pre>
   var number = 1337;
   var date = new Date();
   var myArr = [number, date, "foo"];

   var str = myArr.toLocaleString(); 

   console.log(str); 
   // 输出 "1337,2015/2/27 下午8:29:04,foo" 
   // 假定运行在中文（zh-CN）环境，北京时区
   </pre>

### join （指定分隔符转换为字符串并返回）
1. str = arr.join() 默认为 ","
2. str = arr.join("") 中间没有字符
3. str = arr.join(separator) 指定分隔符
4. 数组元素转换为字符串，再用分隔符链接；

#三、es5方法


##1. 位置方法
**类数组对象无length属性不可用**

###indexof （首个出现在数组中的索引）
1. arr.indexOf(searchElement[, fromIndex（开始查找位置，默认为0） = 0]);未找到返回-1
2. fromIndex可为负值，-n表示从倒数第n个元素开始向后查找（包括此索引位置）
3. 采用严格等于'==='
4. ie(6-8)不兼容

### lastIndexOf  （最后一次出现在数组中的索引）
1. 逆向查找，从最后一个向前找，
2. fromIndex可为负值，-n表示从倒数第n个元素开始向前查


##2. 遍历方法
 **遍历的元素范围在第一次调用时就确定，调用之后添加到数组中的不会被callback访问到；修改后的值可以访问到；被删除的元素和未被赋值的元素（undefined元素）不被访问.**

### forEach （为每个元素执行回调函数 返回undefined）
1. array.forEach(callback[, thisArg])thisArg可选，可以当做fn函数内的this对象
2. 按升序为数组中含有效值的每一项执行一次，返callback函数，**返回undefined所以不可链式调用**
3. **无法终止或跳出循环，**除非抛出异常，如果需要则应使用循环。
4. 数组迭代是被修改，会导致一些元素被跳过,
   <pre>
    var words = ["one", "two", "three", "four"];
    words.forEach(function(word) {
   console.log(word);
   if (word === "two") {
     words.shift();
   }
    });
    // one
    // two
    // four
    </pre>

5. 对象复制函数 
   <pre>
    function copy(obj) {
   var copy = Object.create(Object.getPrototypeOf(obj));
   var propNames = Object.getOwnPropertyNames(obj);

   propNames.forEach(function(name) {
     var desc = Object.getOwnPropertyDescriptor(obj, name);
     Object.defineProperty(copy, name, desc);
   });

   return copy;
    }

 var obj1 = { a: 1, b: 2 };
 var obj2 = copy(obj1); // obj2 looks like obj1 now
 </pre>

### map  (返回[回调函数返回值]组成的新数组)
1. array.map(callback, this)
2. **callback函数返回操作后的值**
3. IE6-8不兼容
   <pre>
    var numbers = [1, 4, 9];
    var roots = numbers.map(Math.sqrt);
    /* roots的值为[1, 2, 3], numbers的值仍为[1, 4, 9] */

 //反转字符串 用slice也可，
 var str = '12345';
 Array.prototype.map.call(str, function(x) {
   return x;
 }).reverse().join(''); 

 // Output: '54321'
 // Bonus: use '===' to test if original string was a palindrome

 //如何去遍历用 querySelectorAll 得到的动态对象集合
 var elems = document.querySelectorAll('select option:checked');
 var values = Array.prototype.map.call(elems, function(obj) {
   return obj.value;
 });
 </pre>

### every (每一项都为true，则返回true)
1. arr.every(callback[, thisArg]) 用来检测每个元素的函数；执行callback使用的this值
2. 
   arr.every(function(value，index，array){
    return value >= 18;
    }) 
3. 类数组对象可使用
4. **callback函数返回（true/false） false停止遍历**

### some (任意一项为true 返回true)
参考every

1. 返回true停止遍历

### filter (**返回值为true**的元素集合的数组)
1. arr.forEach(callback(currentValue,index,array){}[,thisArg])
2. 类数组对象可使用 **无length属性不可用，返回空数组**
3. <pre>
   function isBigEnough(element) {
   return element >= 10;
    }
    var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
    // filtered is [12, 130, 44]
    </pre>
4. callback函数返回true/false

### reduce (对累加器和数组中的每个值[从左到右]应用函数，并返回累加结果，减少为一个值)
1. reduce(callback,[initialValue])
2. function callbackfn(accumulator,currentValue,index,array){**return accumulator**}**需返回累加器**

 > accumulator:上一次调用回调函数的值，或者提供的初始值<br>
 >  currentvalue：数组目前被处理的数组项<br>
 >  index：当前数组项中的索引值<br>
 >  array：调用reduce方法的数组<br>
 >  initialValue:第一次调用callback的第一个参数

3. **第一次执行函数时若有initialValue则accumulator为此参数，currentValue为数组中第一个值；若没有，则accumulator为数组中第一个值，currentValue为数组中第二个值**
4. 如果数组为空，且没有提供initialValue 会抛出TypeError；
5. 如果数组仅有一个元素（无论任何位置），并且没有提供initialValue。或者提供initialValue数组为空，不执行callback函数，返回唯一值；
6. 
   <pre>
    //加和
    var arr = [0,1,2,3,4]; 
    arr.reduce((accumulator,currentValue,index,array) =>{ return preValue +curValue; },10); 
    //数组扁平化
    var flattened = [[0, 1], [2, 3], [4, 5]].reduce( ( acc, cur ) => acc.concat(cur), [] );
    //计算各个值出现次数
    var countedNames = names.reduce(function(allNames, name) { 
   if (name in allNames) {
     allNames[name]++;
   }else {
     allNames[name] = 1;
   }
   return allNames;
    }, {});
    </pre>

### reduceRight（从右往左）
hh



#四、es6方法


###copyWithin（浅拷贝数组的一部分元素到同一数组的不同位置，不改变数组大小，返回数组）
1. arr.copyWithin(target[, start[, end]])
2. target：复制到该位置 start：开始复制元素的起始位置（默认值0） end：复制元素的结束位置（默认值length）
3. **arr.length大小不会改变,超出的部分复制的序列会直接舍弃**
   <pre>
   [1, 2, 3, 4, 5].copyWithin(-2);
   // [1, 2, 3, 1, 2]

   [1, 2, 3, 4, 5].copyWithin(0, 3);
   // [4, 5, 3, 4, 5]

   [1, 2, 3, 4, 5].copyWithin(0, 3, 4);
   // [4, 2, 3, 4, 5]

   [1, 2, 3, 4, 5].copyWithin(0, -2, -1);
   // [4, 2, 3, 4, 5]

   [].copyWithin.call({length: 5, 3: 1}, 0, 3);
   // {0: 1, 3: 1, length: 5}
     </pre>

###fill （将数组指定索引区间填充指定值，返回修改后的数组）
1. arr.fill(value, start, end)[start,end)
2. start和end参数是可选的，默认值分别为0和length属性

 <pre>
 [1, 2, 3].fill(4)            // [4, 4, 4]
 [1, 2, 3].fill(4, 1)         // [1, 4, 4]
 [1, 2, 3].fill(4, 1, 2)      // [1, 4, 3]
 [1, 2, 3].fill(4, 1, 1)      // [1, 2, 3]
 [1, 2, 3].fill(4, -3, -2)    // [4, 2, 3]
 [1, 2, 3].fill(4, NaN, NaN)  // [1, 2, 3]
 Array(3).fill(4);            // [4, 4, 4]
 [].fill.call({length: 3}, 4) // {0: 4, 1: 4, 2: 4, length: 3}
 </pre>
###find （返回满足提供的函数的第一个元素的值，未找到返回undefined）
1. 见findindex

###findIndex （返回满足提供的函数的第一个元素的索引  未找到返回-1 ）
1. arr.findIndex(callback[, thisArg])
2. callback返回true/false，callback返回true立刻停止迭代，返回元素索引，
3. 回调函数只有在数组的索引被分配值的时候才会被调用，若是索引被删除或者没有被赋值将不会被调用

###includes（判断数组是否包含某值，true/false）
1. arr.includes(searchElement, fromIndex[可选，默认为0])
2. fromIndex大于数组，返回false，不搜索；为负值，绝对值大于length，搜索整个数组
3. 类数组可用
4. 不兼容ie

###keys （返回一个新的Array Iterator对象，该对象包含数组中每个索引的键）
1. 也包含没有对应元素的索引

###values （返回一个新的Array Iterator对象，该对象包含数组中每个索引的值）
	<pre>
	for of迭代方式
	var arr = ['w', 'y', 'k', 'o', 'p'];
	var eArr = arr.values();
	// 您的浏览器必须支持 for..of 循环
	// 以及 let —— 将变量作用域限定在 for 循环中
	for (let letter of eArr) {
	  console.log(letter);
	}
	另一种迭代方式next()
	console.log(eArr.next().value); // w
	console.log(eArr.next().value); // y
	console.log(eArr.next().value); // k
	console.log(eArr.next().value); // o
	console.log(eArr.next().value); // p
	</pre>


#四、方法总结


##改变数组
* indexOf
* lastIndexOf
* toString
* join
* concat
* slice
* some
* forEach 可以改变而已
* every
* filter
* map（被废弃）
* reduce
* reduceRight
##不改变数组
* pop
* posh
* shift
* unshift
* splice
* sort
* reverse

#五、判断是否是array对象
##1. arr instanceOf Array (true/false) (查找整个原型链)
object instanceOf constructor

检测一个对象的原型链是否 指向 constructor的原型对象,
<pre>
Object.getPrototypeOf(o) === constructor.prototype
</pre>
##2. arr.constructor === Array
##3. 当涉及到iframe时，会发生
取得对象的一个内部属性，再根据这个属性，返回一个类似于[object Array]的字符串作为结果
<pre>
function isArrayFn (o) { 
    return Object.prototype.toString.call(o) === '[object Array]'; 
} 
</pre>
##4.Array.isArray() ie8+
##5. 综上
<pre>
function isArrayFn(value){ 
    if (typeof Array.isArray === "function") { 
        return Array.isArray(value); 
    }else{ 
        return Object.prototype.toString.call(value) === "[object Array]"; 
} 
}
</pre>

#六、类数组对象
##1.定义
* 拥有length属性，其他属性（索引）为非负整数（对象中的索引被当做字符串来处理，这里可以当做非负整数串理解）
* 不具有push、foreach、indexof等数组对象具有的方法
##2.示例
###
<pre>
var a = {'1':'gg','2':'love','4':'meimei',length:5};
Array.prototype.join.call(a,'+');//'+gg+love++meimei'
</pre>
### arguments对象
### dom方法返回结果
### 类数组函数（函数也有length属性）
##3.类数组判断
<pre>
function isArrayLike(o) {
    if (o && 
    	typeof o === 'object' && 
    	isFinite(o.length) &&
    	o.length>=0 &&
    	o.length< 4294967296) &&//2的32次方
    	return true;
    else
    	return false;
}
</pre>
##4. 类数组表现（Array.prototype.function.call/apply）
<pre>
var a = {'0':'a', '1':'b', '2':'c', length:3};  // An array-like object
Array.prototype.join.call(a, '+'');  // => 'a+b+c'
Array.prototype.slice.call(a, 0);   // => ['a','b','c']: true array copy
Array.prototype.map.call(a, function(x) { 
    return x.toUpperCase();
});                                 // => ['A','B','C']:
</pre>
##5. 类数组对象转化为数组
####Array.prototype.slice.call(arguments,0)之后就可以使用数组方法了
####IE9以前的版本(for循环遍历)
<pre>
// 伪数组转化成数组
var makeArray = function(obj) {
    if (!obj || obj.length === 0) {
        return [];
    }
    // 非伪类对象，直接返回最好
    if (!obj.length) {
        return obj;
    }
    // 针对IE8以前 DOM的COM实现
    try {
        return [].slice.call(obj);
    } catch (e) {
        var i = 0,
            j = obj.length,
            res = [];
        for (; i < j; i++) {
            res.push(obj[i]);
        }
        return res;
    }

};
</pre>
##6. 类数组对象的实现原理
###万物皆对象
>数组和函数的本质是对象，所有对象都可以调用对象的方法，使用点和方括号访问属性
###鸭子类型
>如果它走起来像鸭子，叫起来像鸭子，那他就是鸭子

对比上述就是只要有length属性，就能当数组用
##7. 类数组对象可使用数组方法原理
###V8push源代码
<pre>
// Appends the arguments to the end of the array and returns the new
// length of the array. See ECMA-262, section 15.4.4.7.
function ArrayPush() {
  // 获取要处理的数组
  var array = TO_OBJECT(this);
  // 获取数组长度
  var n = TO_LENGTH(array.length);
  // 获取函数参数长度
  var m = arguments.length;
  for (var i = 0; i < m; i++) {
    // 将函数参数push进数组
    array[i+n] = arguments[i];
  }
  // 修正数组长度
  var new_length = n + m;
  array.length = new_length;
  // 返回值是数组的长度
  return new_length;
}
</pre>