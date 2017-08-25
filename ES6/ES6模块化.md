##用法	

	export default function crc32() { // 输出
	  // ...
	}
	
	import crc32 from 'crc32'; // 输入
	
	// 第二组
	export function crc32() { // 输出
	  // ...
	};
	
	import {crc32} from 'crc32'; // 输入

##
	function add(x, y) {
	  return x * y;
	}
	export {add as default};
	// 等同于
	// export default add;
	
	// app.js
	import { default as xxx } from 'modules';
	// 等同于
	// import xxx from 'modules';

##
	import('./myModule.js')
	.then(({export1, export2}) => {
	  // ...·
	});

##同时加载多个模块
	Promise.all([
	  import('./module1.js'),
	  import('./module2.js'),
	  import('./module3.js'),
	])
	.then(([module1, module2, module3]) => {
	   ···
	});