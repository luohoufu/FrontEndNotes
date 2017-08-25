#一、封装
js是基于对象的语言，所有的语言都是对象牡丹石不是真正的面向对象编程，因为语法中没有class；因此需要把属性和方法封装成对象，甚至从原型对象生成一个实例对象
##1. 生成实例对象的原始模式
##2. 原始模式的改进
	　function Cat(name,color) {
	　　　　return {
	　　　　　　name:name,
	　　　　　　color:color
	　　　　}
	　　}
	var cat1 = Cat('','')
	var cat2 = Cat('','')

这种模式实际是在调用函数，cat1,2之间没有内在联系，无法反应他们是一个原型对象的实例
##3. 构造函数模式
构造函数即为一个普通的函数，内部使用了this变量，this绑定在实例对象上。此时cat1上有一个constructor属性，指向构造函数。

cat1 instanceof cat//true 验证原型对象和实例对象之间的关系 

	　function Cat(name,color){
	　　　　this.name=name;
	　　　　this.color=color;
	　　}

	　　var cat1 = new Cat("大毛","黄色");
	　　var cat2 = new Cat("二毛","黑色");
	　　alert(cat1.name); // 大毛
	　　alert(cat1.color); // 黄色
##4. 构造函数的问题
生成的两个实例，属性和方法会重复创建浪费内存

	alert(cat1.eat == cat2.eat); //false

因此出现原型模式
##5. prototype模式（最靠谱！！）
js的构造函数都有一个prototype属性，指向原型对象，这意味着这个对象所有的属性和方法，都会被构造函数的实例继承。

因此，可
 以将不变的属性和方法直接定义在prototype对
 象上

	　function Cat(name,color){
 	 　		this.name = name;
	　　　　this.color = color;

	　　}
	　　Cat.prototype.type = "猫科动物";
	　　Cat.prototype.eat = function(){alert("吃老鼠")};

	　var cat1 = new Cat("大毛","黄色");
	　　var cat2 = new Cat("二毛","黑色");
	　　alert(cat1.type); // 猫科动物
	　　cat1.eat(); // 吃老鼠
	
	alert(cat1.eat == cat2.eat); //true

##6. prototype模式的验证方法

### isPrototypeOf
判断某个prototype对象和某个实例之间的关系

	Cat.prototype.isProtoTypeOf(cat1)
### hasOwnProperty
判断每个实例对象是否属于本地还是继承属性 true
### in运算符
用来判断某个实例是否含有某个属性，不管是不是本地属性

	alert("name" in cat1); // true
	alert("type" in cat1); // true



还可以遍历某个对象的所有属性

	for(var prop in cat1) { alert("cat1["+pr**op+"]="+cat1[prop])#	


#二、构造函数继承

##1.构造函数绑定（子对象构造函数时在子对象中执行构造函数）
将父对象的构造函数绑定在子对象上，即在子对象构造函数时在子对象中执行

	Anamal并传入this
	　　function Cat(name,color){
	　　　　Animal.apply(this, arguments);
	　　　　this.name = name;
	　　　　this.color = color;
	　　}

##2. prototype模式 （构造函数指向原型构造函数的实例对象）
子对象的构造函数的prototype指向Animal的实例对象，那么子对象的所有实例都可以继承Animal

	　　Cat.prototype = new Animal();
	　　Cat.prototype.constructor = Cat;
	　　var cat1 = new Cat("大毛","黄色");
	　　alert(cat1.species); // 动物

**将Cat的prototype对象指向了Animal的实例，所以Constructor指向Animal，所以加了一句，Cat.prototype.constructor = Cat**

##直接继承prototype（！不靠谱！）
对上一个方法的改进，Animal将不变的属性和方法写入prototype，可以直接继承符对象的prototype

	　function Animal(){ }
	　　Animal.prototype.species = "动物";
	　　Cat.prototype = Animal.prototype;
	　　Cat.prototype.constructor = Cat;
	　　var cat1 = new Cat("大毛","黄色");
	　　alert(cat1.species); // 动物

**缺点：Cat.prototype和 Animal.prototype 同时指向同一个对象，则任何对Cat的修改都会反映到Animal.prototype**

所以上面的第二行代码有问题，修改了Animal的原型的consstructor属性
##3. 利用空对象作为中介
由于上个的缺点，利用空对象作为中介

	　function extend(Child, Parent) {
	
	　　　　var F = function(){};
	　　　　F.prototype = Parent.prototype;
	　　　　Child.prototype = new F();
	　　　　Child.prototype.constructor = Child;
	　　　　Child.uber = Parent.prototype;
	　　}

##4. 拷贝继承(改成深度拷贝即可)

	　　function extend2(Child, Parent) {
	　　　　var p = Parent.prototype;
	　　　　var c = Child.prototype;
	　　　　for (var i in p) {
	　　　　　　c[i] = p[i];
	　　　　　　}
	　　　　c.uber = p;
	　　}


#三、非构造函数继承
两个对象继承
##1.object方法
先给子对象制定构造函数，将构造函数的原型指向父对象，再在子对象天剑属性

	function Doctor() {}
	Doctor.prototype = Chinese;
	var doctor = new Doctor();
	doctor.career = 'yisheng'

##2.浅拷贝
父对象修改方法，会影响子对象

	function extendCopy(p) {
	　　　　var c = {};
	　　　　for (var i in p) { 
	　　　　　　c[i] = p[i];
	　　　　}
	　　　　c.uber = p;
	　　　　return c;
	　　}
##3.深拷贝
递归深拷贝

	function deepCopy(p, c) {
	　　　　var c = c || {};
	　　　　for (var i in p) {
	　　　　　　if (typeof p[i] === 'object') {
	　　　　　　　　c[i] = (p[i].constructor === Array) ? [] : {};
	　　　　　　　　deepCopy(p[i], c[i]);
	　　　　　　} else {
	　　　　　　　　　c[i] = p[i];
	　　　　　　}
	　　　　}
	　　　　return c;
	　　}