#开始

	import Vue from 'vue'
	import App from './App'
	import VueRouter from 'vue-router'
	import goods from './components/goods/goods'
	
	// 安装vueRouter
	Vue.use(VueRouter)
	
	let router = new VueRouter({
	  routes: [{
	    path: '/goods',
	    component: goods
	  }]
	})
	
	/* eslint-disable no-new */
	new Vue({
	  el: '#app',
	  router: router,
	  template: '<App/>',
	  components: { App }
	})


##vue-router设置router-link选中高亮

	let router = new VueRouter({
	  linkActiveClass: 'active', //指定active时，在a标签中增加的类，并在css中写入
	  routes: [{
	    path: '/goods',
	    component: goods
	  }, {
	    path: '/ratings',
	    component: ratings
	  }, {
	    path: '/seller',
	    component: seller
	  }
	  ]
	})


