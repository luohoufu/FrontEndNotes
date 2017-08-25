##声明式渲染
除了绑定插入式的文本内容，还可以采用v-bind

	<div id="app-2">
	  <span v-bind:title="message">
	    Hover your mouse over me for a few seconds to see my dynamically bound title!
	  </span>
	</div>
	
	var app2 = new Vue({
	  el: '#app-2',
	  data: {
	    message: 'You loaded this page on ' + new Date()
	  }
	})

v-bind属性被称为指令，指令带有前缀v-，在渲染过的DOM上应用特殊的响应式行为，这个指令的简单含义是说：讲这个元素节点的title属性和vue实例的message属性绑定到一起

##条件与循环
	<p v-if="seen">now you see</p>
	var app = new Vue({
		el:'',
		data:{
		  seen:true
		}
	})

只有v-if绑定的为true时才显示

v-for 循环加载	
	<li v-for="todo in todos">
	{{todo.text}}
	</li>
	
	var app4 = new Vue({
	  el: '#app-4',
	  data: {
	    todos: [
	      { text: 'Learn JavaScript' },
	      { text: 'Learn Vue' },
	      { text: 'Build something awesome' }
	    ]
	  }
	})

##处理用户输入
###使用v-on指令绑定监听事件

	<button v-on:click="method">reverse</button>
	var app5 = new Vue({
	  el: '#app-5',
	  data: {
	    message: 'Hello Vue.js!'
	  },
	  methods: {
	    reverseMessage: function () {
	      this.message = this.message.split('').reverse().join('')
	    }
	  }
	})

###v-model指令，表单输入和应用状态中做双向数据绑定

	<div id="app-6">
	  <p>{{ message }}</p>
	  <input v-model="message">
	</div>
	var app6 = new Vue({
	  el: '#app-6',
	  data: {
	    message: 'Hello Vue!'
	  }
	})

##用组件构建应用

##生命周期

##props   用来接收父组件的数据

> 可以是简单的数组，或者使用对象作为替代

​	

```java script
exports default {
  props: {
  //类型检测 其他验证
    seller: {
	   type: Object,
	   default: 0,
	   required: true,
	   validator: function (value) {
         return value >= 0
	   }
	}
  }
}
```

## 选项/生命周期钩子

![](http://upload-images.jianshu.io/upload_images/1627906-19fafa634c837a20.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* beforeCreate创建前
* created创建后
* beforeMount挂载前
* mounted挂载后
* beforeUpdate更新前后
* updated
* beforeDestroy销毁前后
* destroyed



##v-show

##transition

##v-on：click   $event



## 模板

```
new Vue({
  el: '#app/.app/body',   //自动挂载  
  						//或者手动挂载new Vue().$mount('#app')
  //模板
  template: '<span>{{message}}</span>',
  data: {
    message: 'Hello Vue.js!'
  },
  computed: {
    fullName: {
      get: function () {
        return this.firstName + ' ' + this.lastName
      },
      set: function (newValue) {
        var names = newValue.split(' ');
        this.firstName = names[0];
        this.lastName = names[names.length - 1];
      }
    }
  },
  watch: {
    msg: foo
  },
  //页面初次加载时，会触发create和mount生命周期钩子
  beforeCreate:function(){log("beforeCreate")},
        //在实例创建之后同步调用。
    //此时实例已经结束解析选项，这意味着已建立：数据绑定，计算属性，方法，watcher/事件回调。
    //但是还没有开始 DOM 编译，$el 还不存在。
    created:function(){log("created")}, 
    修改data的值时会触发destroy，分别触发update destroy生命周期钩子
  beforeMount:function(){log("beforeMount")},    mounted:function(){log("mounted")}, 
  beforeUpdate:function(){log("beofreUpdate")}, updated:function(){log("updated")}, 
  beforeDestroy:function(){log("beforeDestroy")}, 		     destroyed:function(){log("destroyed")}

  methods: {
    foo: function(){
    }
  },
  events: {
    getFullName:function(){ console.log("method exec") 	    	return this.firstName + " " + this.lastName ; }
  }，
  computed:{ 
  	fullName:{ 
  		get:function(){ 
  			return this.firstName + " " + this.lastName; 
  		}, 		
  		set:function(newVal){ 
  			var names = newVal.split(" "); 
  			this.firstName = names[0]; 
  			this.lastName = names[names.length-1]; 
  		} 
  	} 
  },
  watch:{ 
     firstName:function(oldval,newVal){ 
     	console.log("wath exec") 
     	alert("firstName has changed"); 
  		} 
  	}
});
conpute的fullname在结果上和method的getFullName效果一样的，根据文档上的说法，计算属性使用缓存，只有计算属性依赖项发生变化时，计算属性才会重新计算，其他时候直接返回缓存中的值，method则总是会执行函数，另外计算属性默认是get模式，同时也可以采用set模式。
<div id="vm3"> 
{{fullName}} 
<br> 
{{getFullName()}}
</div>

另外当数据变化时来响应数据变化，就需要用到$watch

```

官网中的说法叫`每个 Vue 实例都会代理其 data 对象里所有的属性`，通俗点说就是data对象是引用类型，其值是该对象的地址，因此可以看到vmData.data中的值变了的话，vm.data的值也会跟着变，同样的道理，vm.data变了也会引起vmData.data的变化。
但是如上面代码所示,在vmData赋值给vue实例之后，再给vmData增加的属性，并不会被vue实例所引用。
也就是vmData.b在vue实例中是不存在的。

最后再使用vm.$destroy()方法，这样会触发"destroy"生命周期钩子
$destroy()方法官方说法叫：Teardown wathcers,child components and event listeners
就是拆卸监控，也就是解除双向绑定，值的变化不会再引起dom的变化