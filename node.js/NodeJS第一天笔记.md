## Node的基本概念

### Node是什么?
	用js语言用来开发后台应用程序，它也是一个平台，和Web平台不一样
	
	Web平台:展示&交互
	
	Node平台:
		1、给浏览器提供数据
			浏览器通过Ajax请求，后台给他的一般是JSON的数据
			访问首页，后台给你返回的就是整个网页(字符串)
		
		2、大量的数据统计
		
	官网:
		https://nodejs.org/en/
		http://nodejs.cn/

### Node和Web平台有啥不一样
	相似点:都是用js语法写的
	不同点:
		功能&职责不一样
		
		语法上面有些不一样
			Node没有dom、bom概念
	
--------------------

## Node.exe的安装以及环境变量配置
	apache.exe  node.exe 服务器软件
	
### 安装方式(两种方式选择其一即可)
	1、双击node-v6.11.2-x64.msi 下一步下一步
	
	2、通过nvm的方式安装(网速要好)
		参考:http://www.jianshu.com/p/07c3456e875a
		
		流程:
			先写在原先通过下一步下一步安装的
			再安装nvm
			通过nvm安装具体的node的版本
			
		常用指令:
			nvm install 版本号(比如6.11.4)
			
			nvm list 查看安装的版本
			
			nvm use 版本号，切换到对应的版本号
		
		注意:
			可以让你的电脑上安装多个node版本，但是只能用一个，一般建议大家安装一个高版本的稳定版本(6.11.4)
			
## 启动方式
	1、找到node安装目录双击node.exe(用得比较少)
	
	2、先到node安装目录，通过命令的方式启动node.exe并且执行js代码，一般写法是这样 node 要执行的js文件(因为文件里面有js代码)
		上面的方式有些繁琐，因为我要执行一个js文件，首先得进入到node的安装目录，然后执行 `node js的文件路径`
		
## 系统环境变量(以在终端里面启动QQ为例)
	1、切换到QQ的执行目录(一般是叫Bin)，在终端里面执行QQ或者QQ.exe，可以启动
		但是上面这种很麻烦，每次启动都有先切换到QQ的执行目录
		
	2、把QQ的执行目录(Bin目录，例如C:\Program Files (x86)\Tencent\QQ\Bin)，也就是能看到QQ.exe的这个目录，把它配置到全局的系统环境变量中
		系统 > 高级系统设置 > 高级 > 环境变量 > 系统变量 > Path
		配置，配置好之后，一定得把终端重启，不然不会生效的
	
## 系统环境变量是如何工作的?(以在终端里面启动QQ为例)
	首先会在当前目录下去找QQ.exe，如果找到了就启动，如果没找到，接下来去系统环境变量中一个一个找，找到了就启动，没有，就报`不是内部或是外部指令`

--------------------

## REPL&模块化开发(CommonJS)

### REPL
	`启动好`的一个Node环境，这个时候我们把这个环境就称之为REPL环境，有了这个环境之后我们就可以运行js代码
	
	退出 按住两次Ctrl+C，或者敲.exit
	
	当我们使用node去执行某个js文件的时候，流程如下
	1、在终端中输入 `node test.js`
	2、开启一个REPL环境
	3、使用node.exe去执行test.js中的代码
	4、在cmd终端中输出一句话 `hello good girl!`
	5、退出REPL环境
	
### 前端里面模块化的演变过程
	1、所有js代码写在一个html中
		维护很麻烦
			
	2、分离，把js代码分到不同的js文件，通过script导入
		全局变量名污染

	3、对象的形式，让我们变量不是全局
		如果对象过多，会创建很多对象
	
	4、沙箱来隔离作用域
		(function (window){
			var window = window
			var a = ""
			
			window.xxx = "xxx"
		})(window)
		
	5、CommonJS规范(服务器端)
		NodeJS --- 同步的方式去加载和导出
		
	6、CMD规范(浏览器端)
		seajs http://yslove.net/seajs/
		
	7、AMD规范(浏览器端)
		requirejs http://requirejs.org/
		
	8、UMD

--------------------

## NodeJS中的模块
	前两天主要是学会使用Node的核心模块或者是从npmjs中下载的第三方模块
	
	NodeJS中核心模块有哪些?
	
		HTTP模块:开启Web服务，处理浏览器请求的
		URL模块:为了明白浏览器的意图
		querystring:处理请求的参数的（GET/POST）
		fs模块:file system
		path模块:文件的路径

--------------------

## 使用HTTP模块开启一个Web服务
	编写代码的步骤见代码
	
	通过`node 文件名`的方式执行里面的代码
	
	执行:`node 01-http.js`做的事情如下
		1、启动REPL环境
		2、执行01-http.js中的代码，开启Web服务，监听浏览器的请求，然后处理，响应
	
	
	书写代码最常见的错误
		Listen eaddrinuse : 给定的地址已经被使用
		listen EADDRNOTAVAIL : http://blog.csdn.net/lintcgirl/article/details/45421333
		
		在监听的时候，把事件的request写错了，也会有这个问题
		
		通过res.end返回的中文的字符串

### 网络三要素
	协议	http 通过访问资源
	IP	定位到哪台机器  域名  DNS解析(政府)
	端口 找到机器上的程序(服务器软件)

--------------------

## es6语法
	ECMAScript 6 === ECMAScript 2015
	参考:http://es6.ruanyifeng.com/
	
### let&const
	let 定义变量
	const 定义常量
	
	===> 取到var
	
	let&const 没有变量提升
	
	开发中，如果不需要改就用const，需要改就用let

### 字符串的一些方法
	includes :	判断字符串中是否包含字符
	startsWith : 是否以什么开头，判断协议头 http:// file://
	endsWith : 是否以什么结尾，判断文件后缀
	repeat
	
	模版字符串【掌握】 ``
		格式化字符串 
		占位和替换的功能

### 箭头函数
	格式
		const 函数名称 = (形参列表) => {
			方法体
		}
		
	个人建议:
		刚开始最好写全，哪怕只有一个参数，方法体中只有一句话，都最好写全，但是省略的写法要看懂

--------------------