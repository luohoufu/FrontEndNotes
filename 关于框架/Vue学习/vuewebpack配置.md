vue-cli
##目录结构

	|--README.md 
	|--build
	|  |--build.js
	|  |--check-version.js
	|  |--dev-client.js
	|  |--dev-server.js
	|  |--utils.js
	|  |--webpack.base.conf.js
	|  |--webpack.dev.conf.js
	|  |--webpack.prod.js
	|--CONFIG
	|  |--dev.env.js
	|  |--index.js
	|  |--prod.env.js
	|--index.html
	|--package.json
	|--src
	|  |--App.vue
	|  |--assets
	|     |--logo.png
	|  |--components
	|     |--hello.vue
	|  |--main.js
	|--static

##入口
package.json中

	"scripts": { 
		"dev": "node build/dev-server.js", 
		"build": "node build/build.js", 
		"lint": "eslint --ext .js,.vue src" 
	}
所以执行npm run dev/build时运行的是node build/dev-server.js和node build/build.js

##dev-server.js
那我们就先看一看dev-server

#开发时的webpack
##npm dev-server
##

##
###module
preloaders:[]
loaders:[{}]