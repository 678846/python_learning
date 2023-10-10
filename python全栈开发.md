@[toc](目录)

## JavaScript

1. js的四种引入方式
   
   ```javascript
   //第一种
   <script>alert('哈哈')</script>
   //第二种
   <script src='文件路径'></script>
   //第三种
   <div inclick='alert(11)'>点我</div>
   //第四种
   <a href="文件路径">点我</a>
   ```
   
2. 变量  var :划分当前变量的作用域

3. 数据类型
   3.1基本数据类型

   ```javascript
   Boolean 布尔  true false
   Number 	数字
   String	字符串 
   	- 单引号
   	- 双引号
   	- 反引号  英文状态下 1 左边的键
   		1. 支持换行
           	var str = 
                   `你
                   好
           		`
   		2. 解析变量
   			var str1 = '中国';
   			var str2 = `我是中国人，我爱你${str1}`;
   			console.log(str2);
   undefined  	未定义类型
   null		可以理解为python中的None
   
   ```

   3.2 引用数据类型

   ```javascript
   object(Array function Date ...)
   1.object对象
   	var obj = object();
   	var obj = {};
   2.Arrary  数组
    	var arr = new Array();
   	var arr = [];
   3.function 
   	function fun() {
           console.log('我是函数')
       };
   	fun();
   ```

   3.3 报错 NaN     Not a Number

   ```
   1. NaN 与任何数字计算都是 NaN 
   2. 与 NaN 比较除了NaN !=NaN 为ture ；其余都是false
   ```

4. 类型的强制转换
   ```javascript
   parseInt 	强制转换为整形  100abc->100
   parseFloat	强制转为浮点型  100->100
   Number 		纯数字类型和布尔类型   '123'->123   false->0
   String		在原来的基础上连边套上引号
   Boolean		7种为false  
   				0 0.0 false NaN null undefined ''
   			{}、[]为ture
   
   自动转化：
   	1. Number + Boolean
   		- 1+ture  //2
   		- 1+false //1
   	2. Number + String
   		- '+'号是拼接 100+'100'->'100100' String
   		- 其他符号是运算(必须是数值) 100-'99'->1 'Number'
   	3. Number + String + Boolean
   		- 100+true-'99' ->2
   		- 100+ture+'99' ->'10199'
   	
   ```

5. 运算符
   ```javascript
   与python大致一样 ++ --
   	++num; 先自增后赋值
   	num++; 先复制后自增
       
   	- === 和 !==  
           1. 比较数据类型
           2. 比较值的大小
          var res=1=='1'	//true
          var res=1==='1'	//false
          
         - and &&  or ||  not !
              
         - 三元(目)运算符
         	- python 真值 if 条件判断 else 错误
           - javascript 表达式？真值:假值
              
   ```


6. 流程控制
   6.1 分支结构

   ```javascript
   /*流程：代码执行的过程
       控制：对流程的把控*/
   //单分支
   if (1) {
       console.log("单分支")
   }
   //双分支
   if (1) {
   
   } else {
       console.log("双分支")
   }
   //多分支
   if (0) {
   
   } else if (1) {
       console.log("多分支")
   } else {
   
   }
   //嵌套分支
   if (1) {
       if (1) {
           console.log("嵌套分支")
       }
   }
   // switch case 结构    条件判断是强判断：值的比较、数据类型的比较
   var week = new Date()
   week.getDay()
   switch (week) {
       case 1:
           console.log("星期一")
           break;
       case 2:
           console.log("星期二")
           break;
       case 3:
           console.log("星期三")
           break;
       case 4:
           console.log("星期四")
           break;
       case 5:
           console.log("星期五")
           break;
       case 6:
           console.log("星期六")
           break;
       default :
           console.log("星期日")
           //break;
   }
   ```

   6.2 循环结构

   ```javascript
   /* for (初始值;条件判断;自增自减的值)*/
   for (var i = 1; i <= 10; i++) {
       console.log(i)
   }
   /* 初始值
       while(条件判断)
       自增自减的值*/
   var j = 1
   while (j <= 10) {
       console.log(j);
       j++;
   }
   arr = [100, 99, 98, 97, 96]
   //for var i in Iterable 获取的是索引
   for (var i in arr) {
       console.log(i)
       console.log(arr[i])
   }
   //获取对象中的键
   obj = {'name': 'liu_dong', 'age': 18}
   for (var i in arr) {
       console.log(i)
   }
   //for var of Iterable 获取的是值
   for (var i of arr) {
       console.log(i)
   }
   //不能遍历对象  报错 TypeError: obj is not iterable
   for (var i of obj) {
       console.log(i)
   }
   ```

   

7. 函数
   7.1 函数创建方式

   ```javascript
   //普通函数
   function fun1() {
       console.log('普通函数');
   }
   
   fun1();
   
   //匿名函数
   (function () {
       console.log('匿名函数');
   }
   )
   //Function 创建（不推荐）
   var fun3 = new Function(console.log('函数'));	//函数体防灾括号里
   fun3();
   
   //闭包函数
   function fun4() {
       var a = 5;
   
       function fun5() {
           console.log(a);
       }
       return fun5
   }
   fun4()();
   /*
       闭包函数特点：
           1.内部函数引用了外部函数的局部变量
           2.外函数的返回值是对内函数的引用
       */
       //箭头函数
   var mynum = (x, y) => {
       return x + y;
   };
   var res = mynum(5, 6)
   console.log(res, '箭头函数')
   
   ```

   7.2 参数

   ```javascript
   //位置参数
   function fun1(a, b) {
       console.log(a, b);
   };
   fun1(1, 2, 3);	//形参实参不匹配不会报错
   //默认参数
   function fun2(a, b ,c= 3) {
       console.log(a, b, c)
   }
   
   fun2(1, 2)
   
   //可变参数   agruments 自动接受所以实参
   function fun3() {
       console.log(arguments);
       for (i of arguments){   //value
           console.log(i);
       }
       ;
       for( i in arguments){	//index
           console.log(i);
       }
       ;
   };
   fun3(1, 2, 3, 4, 5, 6)   
   
   // 没有关键字参数
   function fun4(a, b, c) {
       console.log(a, b, c)
   };
   fun4(c = 1, b = 2, a = 3);
   
   ```

   7.3 函数调用

   ```javascript
   //正常调用
   function fun1(){
       console.log("正常调用")
   }
   fun1();
   //函数的立即执行
   function fun2() {
       console.log("立即执行")
   }
   //匿名函数的立即执行
   (function() {
       console.log("匿名函数立即执行")
   })()
   //其他方式的立即执行
   ~function() {
       console.log("其他方式立即执行")
   }();
   !function() {
       console.log("其他方式立即执行")
   }();
   //js 有预加载机制：提前把函数加载到内存中，然后代码整体运行   匿名函数没有预加载，先定义后加载
   ```

   