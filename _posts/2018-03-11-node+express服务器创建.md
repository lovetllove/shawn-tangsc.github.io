---
layout: post
title: "node+express 完整服务器搭建"
date: 2018-03-11
description: "node+express 完整服务器搭建"
tag: nodejs
---

##node+express 完整服务器搭建

###一.初始化node+express服务器
+ 首先初始化express项目
  
	1. 全局安装两个模块

		```
		npm install express -g
		npm install express-generator -g
		```
	
	2. 生成express 基本框架

		```
		express yourProjectName
		```
		
	3. 添加依赖 
		
		```
		cd yourProjectName
		npm install
		```	
	4. 启动服务器
		
		```
		npm start
		```
+ 这个时候你已经基本运行初始化完了一个node+express的项目，你只要执行npm start 就可以在浏览器初步访问[http://localhost:3000/](http://localhost:3000/) 了。

+ 同时，你可以看到一个目录为：
	>bin
	>> www // 项目入口文件
	>node_moduls //项目所需模块
	
	>public // 静态资源，比如图片，js,css之类
	
	>routes //路由文件夹
	
	>views //页面文件，根据你选择渲染引擎来选择放什么后缀的文件，默认是jada文件
	
	>app.js //项目中需要的中间件等基本配置
	
	>package.json //定义项目的基本配置信息等，包括项目所需的依赖和版本号。
  
###二.重新选择页面渲染引擎，让项目可以直接家在html页面

+ 在项目根目录执行

	```
	npm install ejs --save
	```
+ 这个时候你就可以在package.json中看到你新增的ejs，（你也可以将jada的删掉）。
+ 下面我们就去app.js中修改配置。
	
	```
	var ejs = require('ejs');
	app.set('views', path.join(__dirname, 'views'));
	app.engine('html', ejs.__express);
	app.set('view engine', 'html');//注意要把之前jada删掉
	```

###三.设置静态资源的目录和读取方式，引入‘serve-static’
+ 添加依赖

	```
	npm install serve-static --save
	
	```
+ 在app.js中添加引用
   
   ```
   const serveStatic = require('serve-static');
   ```

+ 替换

	```
	app.use(express.static(path.join(__dirname, 'public')));
	```
	为
	
	```
	app.use(serveStatic(path.resolve(__dirname, './views')));
	```
+ 这样，静态资源在文件中的引用就可以使用相对路径而不是必须是绝对路径，你就可以把你的静态资源放在views里面了，而且页面上可以用相对路径引用静态资源。

+ serve-static 还有很多很有用的地方，这里我就不赘述了，详情可以参考[serve-static](https://www.npmjs.com/package/serve-static).

###四，配置log4js,([参考博文](http://blog.csdn.net/q36835109/article/details/52458769?utm_source=itdadao&utm_medium=referral))

+ 新建一个log4js的文件

	```
	/**
	 * Created by tangsicheng on 2018/2/10.
	 */
	var log4js = require('log4js');
	
	log4js.configure({
	    "appenders": {
	        "access": {
	            "type": "dateFile",
	            "filename": "log/access.log",
	            "pattern": "-yyyy-MM-dd",
	            "category": "http"
	        },
	        "app": {
	            "type": "file",
	            "filename": "log/app.log",
	            "maxLogSize": 10485760,
	            "pattern": "-yyyy-MM-dd",
	            "numBackups": 3
	        },
	        "errorFile": {
	            "type": "file",
	            "pattern": "-yyyy-MM-dd",
	            "filename": "log/errors.log"
	        },
	        "errors": {
	            "type": "logLevelFilter",
	            "level": "ERROR",
	            "appender": "errorFile"
	        },
	        "stdout": {//控制台输出
	            "type": 'stdout'
	        },
	    },
	    "categories": {
	        "default": { "appenders": [ "app", "errors", "stdout" ], "level": "INFO" },
	        "http": { "appenders": [ "access"], "level": "INFO" }
	    }
	});
	// log4js.configure(require('./log4js.json'));
	
	var logger = log4js.getLogger('logger');
	
	exports.logger = logger;
	
	exports.use = function(app){
	    app.use(log4js.connectLogger(logger, {level:'auto', format:':method :url response-time=:response-time ms'}));
	}
	```
+ 编辑app.js

	```
	var logger = require('morgan');
	var log4js = require('./config/log4js')
	var app = express();
	log4js.use(app);
	app.use(logger('dev'));
	```	
> 一定要注意顺序!




