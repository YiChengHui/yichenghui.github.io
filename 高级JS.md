## <center>一
1.	in 
		1. 判断对象中 是否有某个值
		2. 判断数组中是否有某个下标
2.	delete关键字  
		1. 删除 没有使用 var 声明的变量
		2. 删除数组 中的元素，但是位置还在，只是值为undefined   ，后面的位置不会往前挪
		3.  删除对象中的属性
3.	js代码的引入方式

		1. <input type="button" onclick="alert('点我干嘛');">
  		2. <a href="javascript:alert('点你咋地')">点一点</a>
  		3.  script支持设置的属性   async
    			async 异步加载 不阻塞后面的解析
				<script async src="./js/longlongTime.js"></script>//这样就不会等先引入的文件执行完在执行后面的代码
4.	true 和false

		console.log([]==![]);//true//可以理解为两个地址值进行比较
		console.log({}==!{});//false
		console.log({}=='[object Object]');//true
			if([]){
			    console.log(2);//会输出2
			  }
		  // 对象 不能直接跟字符串比
		  // js进行比较的时候 内部 会偷偷的调用
		  // 先调用 valueOf 如果还是无法比较 会再次调用 toString()
5.	数据
-	复杂数据类型 引用类型
		  引用类型赋值特点 赋值的是地址
		  赋值之后 多个变量 会指向 同一个对象 修改某一个变量的值 其他指向这个对象的变量 也会受影响
		  赋值 是地址 person1 跟person2 都指向 同一个对象
-	值类型的 赋值特点
			  拷贝赋值
			  js中 基本数据类型 都是 值类型
6.	异常捕获

	      在可能会出错的代码的 外部 使用 语法包裹起来
	      让代码 哪怕有错 也是能够运行
	    语法
	      try{
	        可能会出错的代码
	        哪怕出错了 后续代码还是会正常执行
	      }
	      catch(data){
	        // 如果出错了 会执行这里的代码
	      }finally{
	        无论包裹的代码是否有问题 都会执行这里面的代码
	        如果有些 无论如何都必须执行的操作 可以放在 finally中
	        比如 释放 变量 person = null;
	      }
		throw{
			手动抛出的错误
			}


>银饰全局变量不具有生命提升
>严格模式：禁止给没有声明过的变量赋值
## 面向对象
	1. 好处：避免全局变量污染
	2.  js中 对象的属性 是可以 动态添加的 这种特性叫做  动态特性
	3.  构造函数
	 	1. 构造函数 return了
		    return 基本数据类型没有影响
		    return 复杂数据类型 覆盖默认的返回值
		2. 不使用new关键字调用
		    构造函数，p 对象 undefined 数组 构造函数
		    不使用new关键字 就相当于普通函数的调用
			 var p = Person('超人','小超人'); // 等同于 window.Person，

	4.	 var dog1 = new Dog('哈士奇');
	 	 var dog2 = new Dog('萨摩耶');
		   function Dog(name){
		    this.name=name,
		    this.bark=function(){
		      console.log(this.name+'wuwuuwuwuw');
		    }
  			}
			console.log(dog1.bark==dog2.bark)//false 
	5.   function Dog(name){
			    this.name=name,
			    this.bark=bark
			  }
			
			function bark(){
			  console.log(this.name+'wuwuuwuwuw');
			}
		console.log(dog1.bark==dog2.bark)//true   
		优化代码 让所有 dog的 bark方法 指向的都是同一个 function
>setinterval()//this 指向window
##原型

	1. 是通过构造函数能够访问的一个属性
	2. Plant.prototype.constructor:指向的是 构造函数本身
	3.   把想要共享的部分 丢到 构造函数的原型中 
  		通过这个构造函数 创建出来的对象 都能够访问原型中的属性
	  // 因为 全部丢在 构造函数中 使用匿名函数 会造成 内存的过分消耗
	  // 原型 把需要实例化的对象 公用的部分 丢到原型中
	//私有的放在构造函数中
	4.  构造函数访问原型
		  属性名是 .__proto__
		  不建议使用 
	5. 注意点
	      可以为实例化的对象 添加跟原型同名的属性 并不会影响原型中的属性
	      通过实例化的对象访问属性时,先找自己身上的,没有在原型中找
	6.  注意点
	      原型可以直接赋值一个新的对象
	      如果直接赋值一个新的对象 没有了 constructor这个属性了
	        原本的原型3角关系 被破坏了 并不会影响我们常规代码的执行
	      如果我们要把这个关系 再建立起来
	        为了保证3角的稳定性 还是建议写上
	7. 下面会报错，
	8.   
		1.    function Person(){}    
		        Person.prototype.sayHi=function(){console.log(this.name)}
		          var person=new Person();
		        person.sayHi();
		这样不会报错，Person原型对象定义了公共的say方法，虽然此举在构造实例之后出现，但因为原型方法在调用之前已经声明，因此之后的每个实例将都拥有该方法。
		2. 下面的写法；person.say方法没有找到，所以报错了
				  function Person(){}    
		        Person.prototype={
		            constructor:Person,
		            name:'lisi',
		            age:14,
		            sayHi:function(){
		                console.log(this.name);
		            }
		        }
		          var person=new Person();
		        person.sayHi();
			当var person = new Person()时，Person.prototype为：Person {}(当然了，内部还有constructor属性),即Person.prototype指向一个空的对象{}。而对于实例person而言，其内部有一个原型链指针proto,该指针指向了Person.prototype指向的对象，即{}。接下来重置了Person的原型对象，使其指向了另外一个对象,即
		Object {say: function}，
		这时person.proto的指向还是没有变，它指向的{}对象里面是没有say方法的，因为报错。

## <center>继承
继承的作用

	  对象A 直接从 另外对象中 获取所有的 属性 方法
	  可以为 对象A 添加他独有的属性 方法 不会影响原来的
1.	原型实现继承

		son.prototype=old;
		old是一个对象；若是一个构造函数，son.prototype=（old new 出来的一个实例对象）
		把对象复制给原型，需要把人为的添加原型三角（手动加constructor）
		  // 把oldLing 赋值给 LittleLing的原型
		  LittleLing.prototype = oldLing;
		  // 人为的添加原型三角
		  LittleLing.prototype.constructor =LittleLing;
2.	原型加混入型：既有继承，还有自己的内容
3.	混入型
		for (var key in oldKing) {
		      littleKing[key] = oldKing[key];
		    }
		    // littleKing 拥有了 oldking的 所有属性
		    console.log(littleKing==oldKing);
4.	Object.create（）方法//（对象）

			ES5 新推出了一个 继承的方法  
			浏览器中 兼容较好的是 ES5  兼容较差的是 ES6
		  new了一个新的对象

		  把 传入 作为 参数的 那个对象 设置给 新创建的这个对象 的原型属性
		  这种语法 其实 不太好体现 继承的过程 为了方便理解 可以看上面的代码
		  var newFruit = Object.create(goodfruit);
		  console.log(newFruit.lookat);
		js中实现继承的常用方法 混入 原型 原型+混入 Object.create
	2. Array 扩展内置原型
		1.   Array.prototype.addInfo = function (place) {
			    console.log('原型中的addInfo')
			    for (var i = 0; i < this.length; i++) {
			      this[i] += '--来自于 ' + place;
			    }
			  }
				// 让数组 有这么一个方法
 				// addInfo(place) 功能是 调用之后 可以为数组中的 所有元素 追加一条信息 
	3. 安全的扩张
		1.  // 希望拥有数组的全部功能 -- 继承的数组
			  var fatherArr = new Array();
			  XMArray.prototype = fatherArr;
		2. // 为数组 添加一个 自己的 功能 addInfo
			  // 因为是添加在 自己创建的构造函数上,所以并不会影响 Array的原型
			  XMArray.prototype.addInfo = function () {
			    console.log('xmArr--调用了原型中的方法');
			  }
		3.  var xmArr1 = new XMArray();
			  xmArr1.push('大明');
			  xmArr1.push('老明');
			  console.log(xmArr1);
			  xmArr1.addInfo();
		4. // 小红 为数组添加addInfo功能
		  // 这个时候 再去 为Array的原型 添加 同名方法 并不会影响 之前 起别名的那个构造函数
		  Array.prototype.addInfo = function () {
		    console.log('数组的原型中的addInfo');
		  }


>var foo = function (){}; var ret = foo();   ret为undefined               
>如果函数在执行时，没有指定返回值。但却用变量接收函数执行后的返回值。相当于定义变量，但没赋初值。那么变量的值为undefined。



## 原型链
	1.    对象有原型    __proto__
		  原型也是对象  __proto__
		  对象有原型    __proto__
		  原型也是对象 
		  .......
		  这种链式结构 这种把多个原型 串联起来 形成的结构 称之为  ---原型链
		  原型链的顶端是什么 Object.prototype的原型
	2.   对象访问属性的 查找规则
		  自己内部找 =>有 
		            =>没有 自己的原型中找 =>有
		                                 =>原型的原型中找 =>有
		                                 =>原型的原型的原型中找 找到Object.prototype为止
		  如果找到 Object.prototype 都没有 报错 或者返回 undefined
		  找的越远性能越差 所以 如果为了性能考虑 会把属性 定义在 对象中
		  如果找到头还没有 其实是浪费了 很多的性能 这个时候 为了避免这种情况 不要去 访问为null的属性
		  主要是自己写代码的时候注意即可
	3. Object.prototype的常见方法
		1.  hasOwnProperty 自己的属性 原型链上不算
		2.   isPrototypeOf 是否是某个对象的原型
			  var objPro = Object.prototype;
			  console.log(objPro.isPrototypeOf(p1));//true
			//isPrototypeOf(p1)   ()里面是实例出来的对象，不能写person(这里输出false)
		3.  toLocaleString
		4.   toString
		5.   valueOf
	4. 
##函数也是对象
1.	函数:直接就可以使用 但是 js中全局环境下添加的函数 其实就是 window对象的 方法
 		 // 方法:对象.出来的
2.	// js中 函数就是对象

		  function Person(name) {
		    this.name = name;
		  }
		  Person.eatFood = function () {
		    console.log('吧唧吧唧嘴,好好吃');
		  }
		  // 通过构造函数来访问
		  Person.eatFood();
		
		  var p = new Person();
		  // p.eatFood();//报错
3.	函数创建的几种方式

		  1. 函数声明
		  function eatNoodle(){
		    console.log('滋遛滋遛');
		  }
		  eatNoodle();
		
		  2. 函数表达式
		  var eatBigSuan = function(){
		    console.log('口感嘎嘣脆');
		  }
		  eatBigSuan();
		
		  3. 使用Function 来创建
		  // 如果之传入一个 字符串 作为函数体
		  // 如果传入 多个字符串 最后一个 作为函数体 之前的 作为 形参
		  // 这种用法 实际开发时 不是很频繁
		  // 面试官问 如何使用Function创建函数 如何设置函数体 形参
		  var eatDumpling = new Function('name','console.log(name+"好吃不过饺子;蘸醋才好吃")');
		  eatDumpling('三鲜饺子');
			4. 使用Function 创建  函数体很长的解决方案
		1. 拼接字符串
		2. 创建函数 多行
			1. var eatDumplings = new Function('name',
			    'for(var i =0;i<10;i++){' +
			    'console.log(name+"好好吃 "+i);' +
			    'console.log("老板再来一碗");' +
			    '}'
			  )
		3. 创建函数 多行 --03 模板的方式来实现
			1. var eatDumplings = new Function('name',document.querySelector('script[type="text/html"]').innerHTML);
		4. 创建函数 多行 --04 如果 js支持 多行字符串呢?
			 // 使用 ``(数字1的左边) 可以写多行字符串 
			  // ECMAScript的 版本6 ES6 
			  var eatDumplings = new Function('name',
			  `for(var i =0;i<100;i++){
			      console.log(name+'好好吃'+i);
			    }
			    console.log("老板,我吃不下了");`
			  );
			  eatDumplings('番茄鸡蛋饺子');
		5. Function 原型关系图
			1. 函数也是 对象
			  构造函数 是对象
			  函数既然是对象 他就是他自己的构造函数
			  所有函数的构造函数都是 Function
		6. 实例成员 p.name
		7. 静态方法(静态属性)   Person.sayHello();
		8. 总结:
		      原型
		        公共的部分 丢原型中
		        实例化出来的对象 都有
		      原型链
		        对象使用属性 方法 会顺着原型链 依次往上找 
		        如果忘记代码 可以试着顺着原型链往上去检索 对应的关键字
		      函数也是对象
		        所以看到 函数.属性 函数.方法 不要诧异
		9. arguments caller
			1. arguments 实参 模拟函数重载
			2. caller
				1.  在哪个函数中 调用的 这个函数
				2.  如果是全局环境下 那么 为null
				3.  如果是在其他函数中 调用 就能够访问到那个函数
			3. 直接通过 函数对象 访问的 属性 静态属性
				length 形参的个数
		10. arguments
			1. 函数的arguments属性 跟 函数内部的 arguments有什么区别?
		      内容是一样的
		      函数.arguments是 定义在 Function原型上的属性
		      第二个是 函数内部的变量
				 console.log(TDDog.arguments == arguments);//false
		11. arguments.callee 函数自身
			1. arguments.callee的使用情境
  			避免 外部 修改了 函数名 指向的 内容 导致无法实现 递归
		12. eval()
			1.  eval这个函数
			  可以把 字符串格式的js代码 转化为 可以执行的js代码
			  相当于 把我们传入的 字符串 变为 普通的js代码 然后放在 调用 eval的 位置
			2. 转化JSON格式的字符串
				1.  var JSONString = '{"name":"泰罗奥特曼","enemy":"小怪兽","enemyName":"小林林"}';
				2. var obj= eval(JSONString);//会报错，
				3. JS中 
					1. 没有 块级作用域 ， 
					2. js会忽略 {} ，
					3. 为了让 js不忽略这个大括号 让js把他们看作是一个整体 (1+1)*2
					4. 可以：a.var obj = eval('(' + JSONString + ')'); 
							b. 'var obj = {"name":"泰罗奥特曼","enemy":"小怪兽","enemyName":"小林林"}'
			3. 优点

			4. 缺点：
				1. 别人可以随意的输入 js代码 并且被解析为 js，eval 慎用



> // eval 能不能解析JSON?  能
  // 如何解析? 两边加上大括号
  // 不加小括号错在哪? 忽略了 两边的 大括号


>在js 中  对象都最终继承自Object.prorotype

![yuanxinglian](img/yuanxinglian.png)


##四、第四天

###一、递归
	  1. 递归是什么
              函数内部  调用这个函数
      2. 如何写一个能够正常执行的递归
        函数内部  调用这个函数
        要有跳出的条件
      3. 递归用来干什么
          用来解决一些算法问题
          用简洁的代码来是写一些复杂的功能
      4. 如何使用递归来实现一些功能
	        递归需要我们分析出规律
	        把规律转化为代码  
	5. example:
		1. function getResult3(n){//斐波那契数列
			    if(n==1||n==2)return 1;
			    return getResult3(n-1) + getResult3(n-2);
			  }
			  console.log(getResult3(100));      
		2. function getResult1(n) {//阶乘
			    if (n == 1) return 1;
			    return getResult1(n - 1) * n;
			  }
		  console.log(getResult1(100));  
		3. function getResult2(n, m) {//求n的m次方
		    if (m == 1) return n;
		    return getResult2(n, m - 1) * n;
		  }
		  console.log(getResult2(100,100));   
		4. 通过递归获取后代元素
			1. 通过not 选择器 console.log(document.querySelectorAll('body *:not(script)'));  
				语法 :not(普通选择器)//对 括号中的 选择器 取反    
			2. 递归获取后代元素：
				 var list=[];
			    function findChildren(ele){
			        var childrenlist=ele.children;
			         for(var i = 0 ; i < childrenlist.length; i++){
			             list.push(childrenlist[i]);
			             findChildren(childrenlist[i])
			           }
			    }
			    findChildren(document.body);
			    console.log(list) 
			3. 自己实现 根据id获取dom元素
				 function getIdChild(id){
			         for(var i = 0 ; i < list.length; i++){
			             if(id==list[i].id){
			                 return list[i];
			             }
			           }
			    }
			    console.log(getIdChild('special'))
			4. 根据class获取 dom数组
				    var classList=[];
				    function getClass(classname){
				         for(var i = 0 ; i < list.length; i++){
				             if(list[i].classList.contains(classname)){
				                 classList.push(list[i]);
				             }
				           }
				    }
				    getClass('first');
				    console.log(classList)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

###二、作用域
	 作用域?
          变量的作用范围
        块级作用域?
          {}围起来的区域
          js中没有块级作用域
          如果有块级作用域 那么 大括号之外 是访问不到 下面的a
        js中 什么玩意 函数可以创建作用域 可以创建作用域?
        变量的查找规则?
          使用变量时,首先会在 代码执行的所在的作用域中查找,找得到 使用找不到,往上一级找,找到 全局作用域为止
        静态作用域(词法作用域)? js是静态作用域
          变量的值是多少,取决于函数定义的位置,跟函数调用的位置没有 半毛钱关系

		  函数可以创建作用域 作用域中可以在定义函数 函数可以创建作用域 作用域中可以定义函数
		  作用域中 可以再次创建作用域 可以循环往复
		  当我们使用某个变量时,会依次逐级的顺着作用域往外找,找到最顶级 window 为止
		  查找的这个过程中,相当于 把这些作用域串起来了,这个结构 我们称之为 作用域链
###三、变量提升
	1. a.预解析阶段，提升 
		b. 变量的声明  函数的声明 到变量所在作用域的 顶端
	2. 函数同名：最后声明的 会把 之前声明的 覆盖
	3. 函数 跟变量同名：
		1. 最终的结果是 我们通过那个名字获得的是 函数
			 drinkWater();
		    var drinkWater = '自来水,最解渴,但是别贪杯哦';
		    function drinkWater() {
		      console.log('咕噜咕噜');
		    }//输出'咕噜咕噜'
		2. 报错
		   var drinkWater = '自来水,最解渴,但是别贪杯哦';
		    function drinkWater() {
		      console.log('咕噜咕噜');
		    }	
 			
 			drinkWater();
	4. 条件式 声明
			1. 变量能够提升,但是需要注意的是 如果 条件式声明 函数 只会提升 声明 函数的 代码 不会提升
  			如果是条件式 语句 中是 函数声明 可以把它看做 var func = function(){}只会提升 var func 赋值语句不会提升
				// bark();
		        console.log(bark);
		        var isDog = true;
		        if (isDog == true) {
		            function bark() {
		                console.log('嗷呜~!~!~!~!~!~!~!~!');
		            }
		        } else {
		            function bark() {
		                console.log('喵喵~!~!~!~!');
		            }
		        }
	5. 变量提升 是分 区域的
		1. 提升到了 变量所在 的 script标签的 顶端之后 不会跨域 script标签
		2. 从上往下执行，所以后面script标签可以访问上面的script标签
	5. 函数表达式 如何提升
		1.  // sing();//会报错，var sing  提升了，所以调用会报错；
			  console.log(sing);//undefined
			  var sing = function () {
			    console.log('叽叽喳喳叫,崩~!!!!!.不叫了');
			  }
>Function.constructor:Function


##五、第五天

###一、闭包
	1.   闭包是什么?
          函数中 返回函数(对象)
          function outer(){
            function inner(){}
            return inner;
          }
          函数能够创建作用域
          函数执行完毕之后 这个函数创建的作用域 不在?在? 
            不在的 会被回收,会被释放
          在函数创建的作用域中 声明的变量 还能够被访问到吗?
        闭包能够干嘛?
          延长变量的 生命周期
          对外提供有限的访问权限
        闭包的缺点?
          会导致内存无法回收  消耗内存
          实际开发中 慎用
	2. 作用：利用闭包 对外提供有限的访问权限，对某些变量提供保护功能  来解决 直接修改的问题
	3.   1.闭包
		    函数内部 返回函数 函数也是对象
		    函数内部 返回对象
		  2.闭包的作用是 
		    对外提供有限的访问权限
		    对某些变量提供保护功能
		  3.如果要在方法中返回多个值
		    return 多次 肯定是不行
		    第一个return 会终结这段代码
		    返回一个对象 把要返回的内容 丢到 对象中 即可
	3. 下面这种写法：可以直接修改 money属性
 		  有一天 有意或者无意的直接修改了 某个属性的值 会造成 逻辑的问题，采用闭包就不会有这种问题
		var moneyBag = {
		    money: 1000,
		    useMoney: function () {
		      this.money -= 100;
		    },
		    earnMoney: function () {
		      this.money += .5;
		    }
		  }

##二、缓存

	1. 缓存是什么?
		      占时保存某些东西,需要使用的时候可以拿出来用
		    缓存的特点?
		      暂时保存某些东西
		      大小有限制
		      如果保存的东西太多了会清除之前的内存
		    常见的缓存?
		      软件缓存? 
		        视频缓存
		        音乐缓存
		            每次都需要去网上下载
		        作用
		        把一些东西占时保存起来,下次需要使用时,直接使用
		        提升用户体验,让原本需要等待很长时间的操作,变得十分的方便快捷
	2.  使用缓存来优化代码
      把一些内容保存在缓存中
      每次要计算时 缓存中是否有
                            -->有  直接使用
                            -->没有 计算结果 保存到缓存中
                              ----------------结果
	3. 闭包实现缓存功能
		 1.缓存数据
          cache = {};
        2.有限的缓存量
          maxCount = 20;
        3.容量用完了之后,删除第一个保存进来数据
          删除保存的数据 delete cache[key]
          使用数组 keys = [] 来保存下标
          每次保存数据
            数据保存到对象中 cache[key] = value
            保存到数组中 keys.push(key);
            如果保存满了
              取出数组的第一个元素 var firstKey = keys[0];
              delete cache[firstKey];
              删除数组的 第一个元素 shift
	4. function createCache_plus() {
	    var cache = {}; // 缓存对象
	    var maxCount = 3; // 最大保存数量
	    var keys = []; // 保存很多键 删除第一条数据时 方便获取
	    return function (key, value) {
	      if (value != undefined) {
	        if (keys.push(key) > maxCount) {
	          delete cache[keys.shift()];
	        }
	        cache[key] = value;
	      } else {
	        return cache[key];
	      }
	    };
	  }
	5.js中函数也是对象
	      最终优化利用了 这个特点吧数据保存在了 返回的命名函数上 节约了一个对象声明
	      优化到了这一步
	        暴露了所有的数据外部可以肆意的修改
	 	function createCache_plus() {//
	    var maxCount = 3; // 最大保存数量
	    var keys = []; // 保存很多键 删除第一条数据时 方便获取
	    return function (key, value) {
	      if (value != undefined) {
	        if (keys.push(key) > maxCount) {
	          delete arguments.callee[keys.shift()];
	        }
	        arguments.callee[key] = value;
	      } else {
	        return arguments.callee[key];
	      }
	    };
	  }
	6. Jquery实现闭包缓存功能代码
		  function createCache() {
	    var keys = [];
	    return function (key, value) {
	      if (keys.push(key + " ") > 50) {
			//jQuery的缓存中 对于 传入的key 额外追加了 " "
			//目的是 避免跟函数对象自身存在的属性重名 造成 属性覆盖的问题
	        // Only keep the most recent entries
	        delete arguments.callee[keys.shift()];
	      }
	      return (arguments.callee[key + " "] = value);
	    };
	  }

##三、自执行函数和沙箱（沙盒模式）
	
	1. 自执行函数
		1. 自执行函数 需要被正常的解析 必须跟之前的代码 有一个明显的分隔，否则就会出现无法正常解析的问题
			(function(){console.log('我调用了')}())
 		 	;(function(){console.log('我也调用了哦')})()
	2. 沙箱：  使用自执行函数实现密闭空间  封闭,这种模式 称之为沙箱
		 1.  (function () {
			    // 缓存对象
			    var cache = {};
			    // 有一些框架 不是使用return 而是直接 对外暴露 这个对象的 访问权限
			    // console.log('自执行函数');
			    // 使用这种方式暴露 一般是 某些框架
			    // 严格告诉你 要用我的这个框架 必须使用哪个对象
			    // 直接访问window 相当于内部世界 直接影响外部世界
			    window.cache = window.$ = cache;
			  })()
		2.    推荐的写法
			  // 形参 等同于 在函数的最顶部 声明了一个变量
			  // 下面这种写法 是外部主动的传递给内部
			  // 方便 代码压缩工具去压缩代码
			  // 压缩的原理是 找到 声明的变量 把名字 变为 var abc = a; vac hahaha = b
			  // 使用下面这种方式 也是可以让我们压缩代码
			  (function (window) {
			    // 缓存对象
			    var cache = {};
			    // 有一些框架 不是使用return 而是直接 对外暴露 这个对象的 访问权限
			    // console.log('自执行函数');
			    // 使用这种方式暴露 一般是 某些框架
			    // 严格告诉你 要用我的这个框架 必须使用哪个对象
			    // 直接访问window 相当于内部世界 直接影响外部世界
			    window.cache = window.$ = cache;
			  })(window)
	3. jQuery的沙箱模式
		  // jQuery的最外层组成
		  // jQuery第二个形参是undefined的含义
		  // 全局环境下 有一个叫做 undefined的变量 默认值 是 undefined
		  // 早期undefined是可以被修改的
		  // 我们希望 undefined就是 undefined 但是undefined可以被修改
		  // 定义了一个叫做undefined的形参 不传入任何的 实参 那么 这个undefined 就是 undefined
		  (function (window, undefined) {//不传第二个参数，undefined就是undefined
		    console.log(undefined);
		    // 声明变量
		    var cache = {};	
		    // 为这个变量增加功能
		    // 对外暴露使用的方式
		    // window.jQuery = window.$ = jQuery;
		    window.cache = window._CACHE = cache;
		  })(window)
	4. 数组的循环方法：forEach  map
		1. forEach
			Array.prototype.myeach=function(fn){
			    for (var i = 0; i < this.length; i++) {
			        fn(this[i],i,this);
			    };
			}
			var arr=[1,2,3,4];
				arr.myeach(function(v,i,arr){
				    console.log(v,i,arr)
				})
		2. map
			Array.prototype.mymap=function(fn){
	    		var data=[];
			    for (var i = 0; i < this.length; i++) {
			        data.push(fn(this[i],i,this));
			    };
			    return data;
			}
			
			var result=arr.mymap(function(v,i,a){
			    console.log(v,i,a);
			    return i+'heheda';
			})
##四、函数的几种调用方式 this是谁

	1. 当做函数开直接调用
		 相当于是省略了 window
		  function eatFood(){
		    console.log(this);
		  }
		  eatFood();// 省略了 window.
	2. 当做方法调用
		  谁点出来的 就是谁
		  var obj = {
		    sayHi:function(){
		      console.log(this);
		    }
		  }
		  obj.sayHi();
		  obj['sayHi']();
	3. 构造函数
		  使用new关键字 调用时 构造函数内部的this 是指向 创建出来的实例化对象
		  var obj = {}; this = obj
		  function Person(){
		    console.log(this);
		  }
		  new Person();

			  function person() {
		            console.log(this);
		            return  1;
		            return  [1,2];
		        }
		       var p= person();
		       console.log(p)
		        var p1=new person();
		        console.log(p1)
	4. 一些问题
		1. 问题1 
			  // 函数的调用情境 是可以更改的
			  var ani = {
			    bark:function(){
			      console.log(this);
			    }
			  }
			  ani.bark();// ani
			  var bark = ani.bark;
			
			  bark();//window
	
	    2. 数组也是对象
			  // 对象[key]() this 就是 对象
			  var arr =[
			    function(){
			      console.log(this);
			    },
			    function(){
			      console.log(this);
			    }
			  ];
			  arr[0](); // this 是arr
	
	   3. 问题3
		  // {}在比较时 作为key是 会调用 toString()
		  // {}.toString() [object Object];
		  var obj2 = {
		    name:'rose'
		  };
		  obj2[{}] = function(){
		    console.log(this);
		  }
		  // this 是谁? 当前这个对象
		  obj2[{}]();
		
		  // 使用点语法来访问 我们[{}] 绑定的 那个函数 
		  // obj2.'{}';//报错
		  obj2['[object Object]']();
		
		  // 复杂数据类型 跟基本数据类型比较时 会调用 valueOf toString方法
		  // 如果把一个对象 直接当做key
		  // obj[{}] 内部 调用{}的 toString方法
		  // 等同于 obj['[object Object]'] = value

##五、jQuery插件写法
	
	1. jQuery.extend
		  // 这种功能一般是为$对象 直接增加可以访问的方法 
		  // 使用频率较低
			  $.extend({
				    sayHello: function () {
				      console.log('你好我是jQuery,这是一个打招呼的插件');
				      console.log(this);
				      console.log(this == $);
				    }
				  })
				  // this 是谁：$
				  $.sayHello();
	2. $.fn.extend   this 是jQuery对象
		$.fn.extend({
		    becomeYellow: function () {
		      console.log('我们是黄种人');
		      // this 是jQuery对象
		      // 因为我们在调用是 是 通过jQuery对象.出来的
		      // $('div').becomeYellow();
		      console.log(this);
		      // 既然this是jQuery对象
		      this.css('backgroundColor', 'yellow');
		      // 为了能够链式编程 还要 返回 this
		      return this;
		    }
		  }) 
	3. 特殊情况  这里this不是jquery对象，而是Dom 对象
		$('body').click(function(){
		     console.log(this);
		     this.innerHTML = '我是Dom';
		   })//知识绑定事件，实际点击时是document.body.onclick
		  jQuery基于js封装的一些函数库
		  $('body').click(function(){console.log(this);});
		  内部 可以理解为 就是 document.body.onclick  = function(){}
		  当我们触发了 body的 点击事件时 等同于 document.body.onclick();
		  所以这里的this 就是 绑定的那个 dom对象

##六、apply()、call()上下文调用模式
	
	1. call()
		1. 动态的改变this
		2.  参数1 是this是谁
  		3.  参数2 ...参数n 传递给 函数的 实参
  		4.  test.call({name:'jack',skill:'you jump i jump'},'icemountain','robot');
	2. apply()
		1. 动态的改变this
		2.  参数1 this是谁
   		3. 参数2 传入一个数组(伪数组),传递给函数的 实参
   		4. 如果是伪数组 length 跟 个数不匹配 不足的使用undefined补齐 如果length较短去掉多余
   		5. 伪数组 {0:'jack',1:'rose',length:2};//length一定要写
   		6. test.apply({name:'rose',skill:'swim'},{0:'jack',1:'rose',length:1});
   		7. test.apply({name:'rose',skill:'swim'},['ice','rose','lilis']);
	3. call(),apply() 注意点
		1. 若什么都不传，内部的this是 window
		2. call(null),apply(null) 内部的this也是 window
		3. 如果传入的是 基本数据类型 把他转化为 其 对应的 包装类型，this 就是 传入的这个基本类型的 包装类型
			 test.call(1); // num
			  test.apply('eatFood');// string
			  test.apply(false);// boolean
	4. apply应用（第一个参数传入调用的那个对象：目的是为了希望this不要发生修改，还是传入之前调用的那个对象）
		1. 准备一个真数组 把伪数组的内容全部都 丢到 真数组
			  realArr.push.apply(realArr, fakeArr);
		2. 计算数组的 最大值
				var max = Math.max.apply(Math,numArr);
	5. call()应用
		1. 使用typeof（复杂数据类型 除了 function 都是 object）
		2. 如何才能够获取真实的类型呢?
		3. 开发时如果要获取更为详细的类型 可以使用 Object.prototype.toString.call(对象(数组,日期等));
			Object.prototype.toString.call([]); 获取详细类型
		4. console.log(Object.prototype.toString.call(obj2));
	6.  自己为所有的对象 都添加一个 可以获取类型的方法call()
			 Object.prototype.heima_toString=function(){
					// 返回的格式 [object Object/Date]
			        var result="[object ";
					//通过对象获取原型 通过原型 获取构造函数的名字
			        result+=this.__proto__.constructor.name;
			        result+="]";
			        return result;
			    }
			    console.log(arr.heima_toString()); 
	7. 上下文模式实现继承
		1.  function Student(){
			    this.skill = '上课睡觉流口水';
			    this.goodAt = '体育';
			  }
			  function StudentKing(){
			    // 继承普通学员的内容//构造函数调用是 this 是创建出来的 那个学霸对象
			    Student.call(this);
			    // 自己的技能
			    this.selfSKill = '这次考得真差';
			  }

##七、沙箱（沙盒）模式/自执行函数

	1. 使用自执行函数实现密闭空间   封闭,这种模式 称之为沙箱
	2. 有一些框架 不是使用return 而是直接 对外暴露 这个对象的 访问权限
	3. 严格告诉你 要用我的这个框架 必须使用哪个对象
	4. 直接访问window 相当于内部世界 直接影响外部世界
    		window.cache = window.$ = cache;
	5.   (function (window) {
			    var cache = {};
			    window.cache = window.$ = cache;
			  })(window)
	6. jquery沙箱模式
		1.  (function (window, undefined) {
		    var cache = {};
		    // window.jQuery = window.$ = jQuery;
		    window.cache = window._CACHE = cache;
		  })(window)
		2. jQuery第二个形参是undefined的含义
			1. 全局环境下 有一个叫做 undefined的变量 默认值 是 undefined
			2. 早期undefined是可以被修改的
			3. 我们希望 undefined就是 undefined 但是undefined可以被修改
			4. 定义了一个叫做undefined的形参 不传入任何的 实参 那么 这个undefined 就是 undefined，有两个形参，但是只传一个参数，所以undedfined 就是undefined
##框架步骤
	1. 面向过程
	2. 函数封装
	3. 抽取到对象中（this.songList）
	4. 构造函数+原型
	5. 闭包保护对象（songList  共有的对象）
		1. 提供有限的访问权限
		2. 因为权限有很多 所以我们返回一个对象
	6. 构造函数+闭包+原型
	7. 构造函数+闭包+原型+自执行函数

>数组中删除某个值， var index=this.songList.indexOf(obj);
                this.songList.splice(index,1);




###

	函数调用：window 
  方法调用：指向调用的对象 
 构造函数调用 
 上下文模式：可以改变


	  function Person(a,age){
		  this.name=a;
		  this.age=age;
		  this.sayhi=function(){
			  console.log(this);
		  }
	  }
	  var p1=new Person('lisi',40)
	  console.log(p1.__proto__==Person.prototype);//t
	  console.log(p1.__proto__.__proto__);//
	  console.log(Object.prototype==Person.prototype);//f
	  console.log(Function.prototype==Person.__proto__);//t
	  console.log(Object.prototype==p1.__proto__.__proto__);//t
	  console.log(({}).__proto__==p1.__proto__.__proto__);//t
	  console.log(({}).prototype==p1.__proto__.__proto__);//f
	  console.log(Object.prototype==Person.prototype.prototype);//f
	  console.log(Object.prototype==Person.prototype.__proto__);//t
	  console.log(({}).__proto__.toString.call([]));



## 补充
	1. 获取类型
		1. 如果把一对大括号赋值给其他变量，或者参与运算，那么大括号就变成了字面对象。
	 console.log(obj.toString()===Object.prototype.toString());//true
        console.log(arr.toString()===Object.prototype.toString());//false
		{}.toString();//错误，代码块不能调用方法
	2. jquery对外暴露的jquer和$ ，调用这两个方法，可以得到jQuery实例对象，jquer和$ 实际上是一个工厂函数；
	3. jQuery实例对象是一个伪数组对象
		var $div=$('<div></div>');
        console.log($div);//[<div></div>];
        console.log(({}).toString.call($div));//[object Object]
	4. 借用数组的push方法给obj按照下标的方式添加属性值，并且会自动维护lenghth属性
		1. [].push.apply(obj,[1,2,3,4]);
	5. 借用数组的pop方法删除obj最后一个属性值
        [].pop.call( obj );
	6. jq入口对不同参数处理的规律：
        1、传入null、undefined、0、NaN、''返回空对象( 即空实例 )
        2、传入字符串，那么需要判断是html片段 还是 其它，
        如果是片段，则创建对应的DOM，然后添加到实例身上；
        否则按照选择器获取页面中的DOM，然后把获取到的DOM添加到实例身上。
        3、如果是数组或许伪数组，那么把每一项分别添加到实例身上。
        4、除了上面的数据类型，剩余的，统一添加到实例身上。



   function fn(){
       num=100;            //num没有声明就直接赋值
   }

   //console.log(num);       //隐式全局变量不具有声明提升，15行会报错

   fn();
   console.log(num);       //num就是一个全局变量

 //ES5的严格模式：禁止给一个没有声明过的变量赋值
    "use strict";
    function f1(){
        str="hello";

    }
    f1();