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


8. js 对象
   ```javascript
   //第一种创建对象
   var obj1 = new Object()
   obj1.name = "刘栋"
   obj1.age = 18
   obj1.eat = function () {
       console.log(obj1.name, '第一种')
   }
   console.log(obj1.name)
   console.log(obj1.age)
   obj1.eat()
   //第二种创建  双引号可加可不加
   var obj2 = {
       "name": '刘栋',
       "age": 18,
       eat: function () {
           console.log(obj1.name, '第二种')
       }
   }
   console.log(obj2.name)
   console.log(obj2.age)
   obj2.eat()
   //注意点
   console.log(obj2["name"])
   console.log(obj2["age"])
   obj2["eat"]()
   
   //第三种创建对象方式
   function Person(name, age) {
       this.name = name;
       this.age = age;
       this.eat = function () {
           console.log(this.name, '第三种')
       }
   }
   
   var obj3 = new Person('刘栋', 18)
   console.log(obj3.name)
   console.log(obj3.age)
   obj3.eat()
   
   //获取键
   for (i in obj1) {
       console.log(i)
   }
   //获取成员对象的值
   with (obj1) {
       console.log(name)
       console.log(age)
       console.log(eat)
   }
   //遍历js对象
   for (i in obj1) {
       console.log(i, typeof (i))
       with (obj1) {
           //将字符串当做代码来执行
           console.log(eval(i))
       }
   }
   
   
   //json 字符串  函数不能传递
   var data = {
       "name": "刘栋",
       "age": 18,
       func: function () {
           console.log("函数")
       }
   }
   
   var res1 = JSON.stringify(data)
   //JSON.stringify 将js对象转化为json格式的字符串
   console.log(res1, typeof (res1))
   //JOSN.parse 将json格式的字符串转换为js对象
   var res2 = JSON.parse(res1)
   console.log(res2, typeof (res2))
   
   ```

9. 字符串对象函数
   ```javascript
     var str = 'to be or not be'
       //字符串长度  length
       console.log(str.length)
       //清除俩边空白符  trim
       console.log(' hh '.trim())
       //获取首次出现的位置  indexOf  未找到返回-1
       console.log(str.indexOf('b'))
       //连接字符串 concat
       console.log('liu'.concat('dong'))
       //截取字符串  slice 顾头不顾尾
       console.log(str.slice(1, 7))
       console.log(str.slice(-5, -1))
       //截取字符串 substr(开始值，截取个数)
       console.log(str.substr(2, 5))
       //拆分字符串 splite
       console.log(str.split(' '))
       //大小写 toUppercase toLowercase
       //正则 /正则表达式/g 全局匹配 i 不区分大小写 m 多行匹配
       //search  返回匹配第一次出现的索引位置  未找到返回-1
       console.log(str.search(/n/))
       //match 匹配返回的数据
       var str2 = "123456 哈哈 123789"
       console.log(str2.match(/\d{6}/))
       console.log(str2.match(/\d{6}/g))
       //字符串替换 replace 默认替换一次
       console.log(str.replace("to", "two"))
       // 全部替换
       console.log("to be or not to be".replace(/to/g,"two"))
   ```

10. 数组对象相关方法

    ```javascript
    //定义一个数组
    var arr1 = new Array();
    console.log(arr1, typeof (arr1), 'arr1');
    var arr2 = []
    console.log(arr2, typeof (arr2), "arr2");
    
    //增
    var arr3 = []
    arr3[0] = 10
    arr3[1] = 11
    arr3[2] = 12
    //js 特点允许在临时索引上插入数据  中间的索引位置为空
    arr3[10] = 100
    console.log(arr3, 'arr3')
    //push 从数组的尾部添加数据
    arr3.push(101)
    console.log(arr3, 'push')
    //unshift 从数组头部添加数据
    arr3.unshift(102)
    console.log(arr3, 'unshift')
    //concat 迭代添加数据
    arr3.concat(["你好", "我好", "她也好"])
    console.log(arr3, 'concat')
    
    //删
    //delete 删除 删除的位置为空，不会向前补位
    var arr4 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    delete arr4[3]
    console.log(arr4, 'delete')
    //pop 从数组后面删除,弹出元素
    var res1 = arr4.pop()
    console.log(res1, 'pop')
    //shift 从数组前面删除
    var res2 = arr4.shift()
    console.log(res2, 'shift')
    //splice(从第几个位置开始,删除几个,[可选的是添加的元素])
    arr4.splice(2, 2, ['刘栋', 'age'])
    console.log(arr4, 'splice')
    
    //改查
    console.log(arr4[5])
    arr4[5]='hh'
    console.log(arr4,'改查')
    
    //join 拼接字符串
    var arr5=["you","can","you","up"];
    console.log(arr5.join("-"),"join")
    
    //数组元素反转
    var arr6=["you","can"];
    console.log(arr6.reverse(),'reverse')
    
    //截取数组的一部分 slice
    var arr7=['德','玛','西','亚']
    console.log(arr7.slice(0,2),'slice')
    console.log(arr7.slice(-2,-1),'slice')
    
    ```

11. 数学相关函数
    ```javascript
    //四舍五入 round
    console.log(2.31, 'round')
    console.log(3.5, 'round')
    console.log(4.2, 'round')
    //最大值 max
    console.log(Math.max(1, 2, 3, 4, 5), 'max')
    //最小值 min
    console.log(Math.min(1, 2, 3, 4, 5), 'min')
    //绝对值 abs
    console.log(Math.abs(-90), 'abs')
    //向下取整 ceil
    console.log(Math.ceil(3.001))
    //向上取整 floor
    console.log(Math.floor(3.999))
    //幂运算 pow
    console.log(Math.pow(3, 2))
    //开方运算 sqrt
    console.log(Math.sqrt(9))
    
    
    // 获取0-1 的随机值
    console.log(Math.ceil(Math.random()))
    // 获取 1-10的随机值
    console.log(Math.ceil(Math.random() * 10))
    //获取 5 到 14 的随机值
    console.log(Math.ceil(Math.random() * (14 - 5 + 1))) + 4
    
    // 随机推导式
    function randomint(n, m) {
        return Math.ceil(Math.random() * (m - n + 1))
    }
    ```

12. BOM对象
    12.1 window对象

    ```javascript
    //BOM browser object model
    //js 中最大的对象 window ：整个浏览器所以的内容和行为都是window的成员
    //输出成员
    console.log(window)
    //弹出警告框
    window.alert('我是警告框')
    //确认弹窗
    var res = window.confirm()
    console.log(res)
    //等待输入框
    var res = window.prompt('请你输入密码')
    console.log(res)
    //关闭浏览器窗口
    window.close()
    
    //获取浏览器的长度和宽度
    console.log(`浏览器窗口内部的宽度${window.innerWidth}`)
    console.log(`浏览器窗口内部的高度${window.innerHeight}`)
    // 页面跳转 _blank 打开新页面  _self 当前页面跳转
    window.open('https://www.google.com','_blank')
    window.open('https://www.google.com','_blank','width=500,height=500')
    // ###定时器
    /*
                # 定时器种类(两种):基于单线程的异步并发程序-协程;
                window.setInterval(函数名,间隔时间(毫秒))   // 定时执行多次任务
                window.setTimeout(函数名,间隔时间(毫秒))    // 定时执行一次任务
    
                window.clearInterval(id号)  // 清除定时器 setInterval
                window.clearTimeout(id号)   // 清除定时器 setTimeout
    
            */
    
    var num = 1
    function func(){
        console.log(`我执行了${num}`);
        num++;
    }
    
    var id1 = window.setInterval(func,1000);
    var id2 = window.setTimeout(func,2000);
    console.log(id1,"id1")
    console.log(id2,"id2")
    console.log("我执行完了....")
    window.clearInterval(id1)
    ```

    12.2 案例 :浏览器页面获取实时时间    定时器+时间对象

    ```javascript
    // 获取节点
    var obj = document.getElementById('clock')
    
    function func() {
        var date = new Date();
        console.log(date)
        //获取年份
        var year = date.getFullYear()
        console.log(date.getFullYear(), '年份')
        //获取月份
        var month = date.getMonth()
        console.log(date.getMonth(), '月份+1')
        //获取天
        var day = date.getDay()
        console.log(date.getDay(), '天')
        //获取小时
        var hour = date.getHours()
        console.log(date.getHours(), '小时')
        //分
        minute = date.getMinutes()
        console.log(date.getMinutes(), '分')
        //秒
        second = date.getSeconds()
        console.log(date.getSeconds(), '秒')
        var time = `${year}年-${month + 1}月-${day}日 ${hour}:${minute}:${second}`
        console.log(time)
        //将时间插入到html中
        obj.innerHTML = time
        //清除定时器
        if (minute == 51) {
            clearInterval(id);
        }
    }
    
    //定时器 返回定时器的id 类似于pid、tid
    var id = window.setInterval(func, 1000)
    ```

    12.3  Navigator 对象

    ```javascript
    //navigator 提供浏览器及操作系统等信息
    console.log(navigator);
    //判断是pc端还是移动端
    console.log(navigator.platform)
    //爬虫程序：浏览器的身份标识
    console.log(navigator.userAgent)
    ```

    12.4 history历史对象

    ```javascript
    function func1() {
        console.log(history);
    }
    
    function func2() {
        history.go(-1);
    }
    
    function func3() {
        //history.go(1);
        history.go(2);
    }
    
    function func4() {
        history.go(0);
    }
    - <body>
        <button onclick="func1()">查看历史记录</button>
    	<button onclick="func2()">跳转到上一页</button>
    	<button onclick="func3()">跳转到下一页</button>
    	<button onclick="func4()">刷新当前页面</button>
         </body>
    ```

    

