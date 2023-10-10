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

   