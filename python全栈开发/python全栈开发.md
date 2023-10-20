## JavaScript

### 1. js的四种引入方式

```javascript
//第一种
<script>alert('哈哈')</script>
//第二种
<script src='文件路径'></script>
//第三种
<div onclick='alert(11)'>点我</div>
//第四种
<a href="文件路径">点我</a>
```

### 2. 变量  var :划分当前变量的作用域

### 3.  数据类型

3.1 基本数据类型

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

### 4. 类型的强制转换

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

### 5. 运算符

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

### 6. 流程控制

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



### 7. 函数

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

### 8. js 对象

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

### 9. 字符串对象函数

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

### 10. 数组对象相关方法

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

### 11. 数学相关函数

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
    return Math.ceil(Math.random() * (m - n + 1))}
```

### 12. BOM对象

12.1 window对象

```javascript
//BOM browser object model  浏览器对象模型
//js 最顶层的对象 window ：整个浏览器所有的内容和行为都是window的成员
1. 输出成员
console.log(window)
2. 弹出警告框
window.alert('我是警告框')
3. 确认弹窗
var res = window.confirm()
console.log(res)
4. 等待输入框
var res = window.prompt('请你输入密码')
console.log(res)
5. 关闭浏览器窗口
window.close()

//获取浏览器的长度和宽度
console.log(`浏览器窗口内部的宽度${window.innerWidth}`)
console.log(`浏览器窗口内部的高度${window.innerHeight}`)
// 页面跳转 _blank 打开新页面  _self 当前页面跳转
window.open('https://www.google.com','_blank')
window.open('https://www.google.com','_blank','width=500,height=500')
// ###定时器

#定时器种类(两种):基于单线程的异步并发程序-协程;
window.setInterval(函数名,间隔时间(毫秒))   // 定时执行多次任务
window.setTimeout(函数名,间隔时间(毫秒))    // 定时执行一次任务

window.clearInterval(id号)  // 清除定时器 setInterval
window.clearTimeout(id号)   // 清除定时器 setTimeout


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

12.2  document 对象

```javascript
DOM 文档对象类型
<div id="box">
    <p class="p1">张三</p>
<p class="p2">李四</p>
<p class="p2">赵刘</p>
<p name="ceshi1"></p>
<p name="ceshi1"></p>
</div>
- 1.节点提取
    //document 获取网页html文档，可以获取网页的所有节点(元素、文档)
    console.log(document)
    1.通过id获取节点对象
    console.log(document.getElementById('box'))
    2.通过类名获取节点对象 返回的数组
    console.log(document.getElementsByClassName('p1'))
    console.log(document.getElementsByClassName('p2'))
    3.通过标签获取节点对象 返回的是数组
    console.log(document.getElementsByTagName('p'))
    4.通过标签的name属性获取节点对象 返回的是数组
    console.log(document.getElementsByName('ceshi1'))
    5.通过css选择器获取对应的元素节点
    //querySelector 获取找到的第一个
    console.log(document.querySelector('.p2'))
    //querySelectorAll 获取所有找到的元素节点
    console.log(document.querySelectorAll('p'))
- 2.节点层级关系
	1.获取html
    console.log(document.documentElement)
    2.子节点
    console.log(document.documentElement.children)
    var tag=document.querySelector('div[id="box"]');
    3.获取下一节点对象
    console.log(tag.nextSibling)
    4.获取下一元素节点对象
    console.log(tag.nextElementSibling)
    5.获取上一节点对象
    console.log(tag.previousSibling)
    6.获取上一元素节点对象
    console.log(tag.previousElementSibling)
    7.获取父节点元素对象
    console.log(tag.parentElement)
	
```

12.3  Navigator 对象

```javascript
1. navigator 提供浏览器及操作系统等信息
console.log(navigator);
2. 判断是pc端还是移动端
console.log(navigator.platform)
3. 爬虫程序：浏览器的身份标识
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

12.5 location 对象

```javascript
<button onclick="fun1()">查看浏览器当前URL信息</button>
<button onclick="fun2()">跳转其他页面</button>
<button onclick="fun3()">刷新页面</button>
<button onclick="fun4()">过一秒刷新页面</button>
1.获取浏览器的URL信息
console.log(location)
2.获取协议
console.log(location.protocol)
3.ip
console.log(location.host)
4.端口
console.log(location.port)
5.路径
console.log(location.pathname)
6.锚点  -描点：超链接的一种，也叫命名锚点通过name定位 # 后面
console.log(location.hash)
7.地址栏参数  ？后面
console.log(location.search)
8. 完整地址
console.log(location.href)

function fun1() {
    document.write(`${location}`)
}

function fun2() {
    location.href = 'https://www.baidu.com/'
    location.assign('https://www.jd.com')
}

function fun3() {
    location.reload()
}

function fun4() {
    setTimeout(fun3, 1000)
}
//每过一秒刷新页面
window.onload = function() {
    fun4()
}
```

12.6 案例 :浏览器页面获取实时时间    定时器+时间对象

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
    1. 将时间插入到html中
    obj.innerHTML = time
    2. 清除定时器
    if (minute == 51) {
        clearInterval(id);
    }
}

定时器 返回定时器的id 类似于pid、tid
var id = window.setInterval(func, 1000)
```

12.7 清除修改内容

```javascript
<button onclick="func1()">修改内容</button>
<button onclick="func2()">清空内容</button>
<p>我是最靓的崽<a href="#">点我查看</a>></p>
    var p =document.querySelector('p')
1. innerHTML 获取节点的所有内容(标签+文本)
2. innerText 获取节点的文本内容
function func1() {
    p.innerText = '我被修改了，<a href="#">点我查看</a>胖头'
    p.innerHTML = '我被修改了，<a href="#">点我查看</a>胖头'
}
function func2() {
    p.innerText = ''
}
```

12. 8 全选、不选、反选

```javascript
<style>  * {margin: 0px;padding: 0px;}
ul {list-style: none;}
#ul1 li {float: left;}
#ul1 li button {width: 80px;height: 30px;}
#ul1 li button:hover {background-color: tan;}
#ul2 {clear: both;}
</style>
<ul id="ul1">
    <li>
    	<button onclick="fun1()">全选</button>
	</li>
	<li>
    	<button onclick="fun2()">不选</button>
	</li>
	<li>
   	 	<button onclick="fun3()">反选</button>
	</li>
</ul>
<ul id="ul2">
	<li><input type="checkbox"/> 看电影</li>
	<li><input type="checkbox"/> 吃面</li>
    <li><input type="checkbox"/> 烫头</li>
    <li><input type="checkbox"/> 跑步</li>
    <li><input type="checkbox"/> 篮球</li>
</ul>

var bt1 = document.querySelector('ul li button:nth-child(1) ')
var bt2 = document.querySelector('ul li button:nth-child(2) ')
var li = document.querySelectorAll('ul li input')
function fun1 () {
    for (i of li) {
        i.checked = true
    }
}
function fun2() {
    for (i of li) {
        i.checked = false
    }
}
function fun3() {
    for (i of li) {
        if (i.checked === true) {
            i.checked = false
        } else if (i.checked === false) {
            i.checked = true
        }
    }
}
```

### 13. js 控制css相关属性

```javascript
<button id="box1" onclick="fun1()">点击我换颜色</button>
<button id="box2" onclick="fun2()">点击我隐藏</button>
<button id="box3" onclick="fun3()">点击我显示</button>
<div class="box" style="width:300px;height:200px;background-color: yellow;font-size:40px;">你好评</div>
var box = document.querySelector(".box")
js 获取css相关属性
第一种 只能获取行内style
console.log(box.style.width)
console.log(box.style['width'])

第二种
console.log(window.getComputedStyle(box))

function fun1() {
    box.style['background-color'] =  'red'
}
function fun2(){
    box.style.display='none'

}
function fun3(){
    box.style.display='block'
}
```

### 14. js 事件

```javascript
<style>
    * {
    margin: 0px;
    padding: 0px;
}

    .box {
        width: 100px;
        height: 100px;
        background: green;
        position: absolute;
        left: 0px;
    }
</style>
<button onclick="fun1()">按钮</button>
<div class="box"></div>

var box = document.querySelector('.box')
console.log(box)
var id;
1.事件的静态绑定
function fun1() {
    将获取到的字符串转化为数字
    var left = parseInt(window.getComputedStyle(box).left)
    全局变量 id
    id = setInterval(function () {
        left += 50;
        box.style.left = `${left}px`;
    }, 1000)

}
2.事件的动态绑定
	2.1 onmouseover 鼠标指针悬停在指定元素上时触发
	box.onmouseover = function(){
    	clearInterval(id);
	}
	2.2 onmouseout  鼠标指针离开指定元素时触发
    box.onmouseout = function(){
        fun1()
    }
```

### 15. 模态框

```javascript
<style>
    .box {
        position:fixed;
        width:100%;
        height:100%;
        top:0px;
        background-color: greenyellow;
        display: none;
    }

.content
{
    border:solid 1px red;
    width:500px;
    height:500px;
    background-color:tan;
    margin:auto;
    margin-top:14%;
}

</style>
<button id="login">登录</button>
<div class="box">
    <div class="content" >
        <span class="close">X</span>
<br />
            <span>账号: <input type="text"  /></span>
<br / >
                <span>密码: <input type="password"  /></span>
</div>
</div>

var box= document.querySelector('.box');
var btn = document.querySelector('#login')
var x = document.querySelector('.close')
btn.onclick = function() {
    box.style.display="block";
}
x.onclick = function() {
    box.style.display="none";
}
```

### 16. ajax异步加载

```javascript
ajax 异步加载（异步的 JavaScript 和 XML）  不刷新页面，也能请求到数据
api 程序调用的接口
1.创建ajax请求对象
var xhr=new XMLHttpRequest();
console.log(xhr);
2. 打开链接
true 异步  false 同步
xhr.open('GET','url',true)
3.发送请求
xhr.send();
readyState 存有 XMLHttpRequest 的状态
    0   请求初始化
    1   与服务器建立连接
    2   接受请求
    3   请求处理
    4   请求完成，响应就绪
    *
4 readyState 变化是就会触发 onreadystatechange事件
服务端状态码为4 客户端状态码为200 表示数据传输完成

xhr.responseText 获取json格式的字符串
xhr.responseHtml 获取xml
```

## jQuery

### label补充

```javascript
label为input输入框定义标注
1.方式一
	<label >用户名:<input type="text"></label>
2.方式二
    <label for = 'password'>密码:</label>
    <input type="password" id="password">
```

### 1.jquery的引用

```javascript
jquery 是原生js的封装
1.本地引入
<script src='jquery路径'></script>
2.外地引入
    bootcdn 中文网开源项目
    <script src='https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.js'></script>
```

### 2.jquery对象和dom对象的相互转化

```
jquery[索引] --> dom
$(dom对象)   -->jquery 

jquery对象和dom对象不能混用
```

### 3.jquery选择器

```javascript
1.语法：$() 得到的是 jquery对象
	$('.id') $('#class_name') $('tag_name')
2.组合选择器
	$('.class_name,#id')
3.层级选择器
	$('tag_name #class_name')
4.属性选择器
	- $()['attr']
	- $() ['attr'='attr_value']
5.表单对象属性选择器
	1.$(':checked') 找到input标签和select下拉选择标签中选择的标签
    2.$(':selected') 找到select下拉框中选中的标签
    3.$(':enable')	找到可操作的标签
    4.$(':disable') 找打不可操作的标签
6.表单选择器
	针对input标签
	1.$(':text')	找到所有type='text'的input标签
	2.$(':radio')	找到所有type='radio'的input标签
	...
7.筛选器方法
	1.父标签 parent()
	2.子标签 children()
	3.直系父辈 parents()
	4.下一个兄弟标签	next()
	5.下面所有兄弟标签	nextAll()
	6.找上一个兄弟标签	prev()
	7.找上面所有兄弟标签	prevAll()    [0]  表示的是上一个兄弟标签
    8.找到不包含自己兄弟标签	Sibling()
	9.找后代标签	find()
	10.取第一个元素	first()  是jquery对象
    11.去最后一个元素	last()	是jquery对象
    12.通过索引jquery对象	 eq(索引值)
	注意： jquery[0] 取的是dom对象，不是jquery对象				
```

### 4.文本内容

```javascript
1.dom对象
    innerText 文本字符串
    innerHTML 代标签的文本字符串
2.jquery对象
    text() 文本字符串
    html() 带标签的文本字符串
```

### 5.类值操作

```javascript
1.addClass()	添加类值
2.removeClass()	删除类值
3.toggleClass()	如果有值则删除,没值则删除
    -> 网站的闪烁操作
setInterval(function(){$('.c1').toggleClass('.c2');},1000)
css优先级
1.后来者居上，相同级别，后面的覆盖前面的
2.inline>id>class>tag>inherit
3.!import 最nb
```

### 6.值操作

```javascript
获取用户输入的内容
1.普通文本框	type='text'
	$('input ..').vai()
2.单选框	type='radio'
	$('input:checked').val() 获取已选中的内容
3.复选框	type='checkbox'
	- $('input:checkbox:checked').val()		不能直接获取所有被勾选上的标签属性value对应的值、，只能获取一个
	循环获取
    	var checkbox = $('input:checkbox:checked')
        - 方式一 
        	for (i of checkbox){
                return i.val()
            }
		- 方式二	each循环获取
        	checkbox.each(function(k,v){
                return k,v  //索引，值
    		})
设置值
1.普通文本框 $('input ..').vai('xxx')
2.单选框	$('input:checked').val(['1'])
3.多选框	$('input:checkbox:checked').val(['1','3'])
```

### 7.创建标签实例

```javascript
<button id="zouni">添加标签</button>
<div id="d1">
    <h1>插入标签</h1>
</div>

1.绑定事件
$('#zouni').click(function(){
   	2.创建标签
    var a =$('<a>',{
        text: '跳转百度',        注意：'一定要添加text属性，要不然会啥也看不见，会很尴尬'
        name:'这是一个a标签' ,
        href:'http://www.baidu.com/',
        id:'liu'})
    3.找到插入标签的位置
    var div = $('#d1')
    4.插入标签
    div.append(a);      //内部后面添加标签
    // div[0].appendChild(a)    原生js方法
})
```

### 8.文档操作

```javascript
<div id="d1">
    <h1>亚洲</h1>
</div>

var div=$('#d1')
1.标签外部的前面添加标签
div.before('<h1>欧美</a>')
2.标签外部后面添加标签
div.after('<h1 >偷拍</h1>')
3.标签内部前面添加标签
div.prepend('<h1>日韩</a>')
4.标签内部后面添加标签
div.append('<h1>素人</a>')

```

### 9.清空和删除

```javascript
1.删除
	$().remove()
2.清空
	$().empty()  相当于$().text=''
```

### 10.事件冒泡

```javascript
事件冒泡：有内到外触发
当父元素和子元素都绑定了点击事件，点击了子元素，子元素和父元素都会触发点击事件
    <style>
        .c1{
            height:200px;
            background-color: red;

        }
        .c2{
            height:100px;
            width:100px;
            background-color: green;
        }
    </style>

$('.c1').click(function(){
    alert('父元素');
})
$('.c2').click(function(){
    alert('子元素')
    return false; //阻止冒泡事件的发生
})

```

### 11.绑定事件

```javascript
1.$().click(function(){})
2.$().on('click',function(){})
```

### 12.克隆

```javascript
<button class="c1">弹弹弹</button>

$('.c1').click(function(){
    var btn = $(this).clone(true)   //clone 默认是不克隆事物的，true表示事件会被绑定上
    $(this).after(btn)
})
```

### 13.事件委托

```javascript
指的是把原本绑定在子元素的事件，委托给父元素，让父元素担当事件监听的职务

<button class="add">保存</button>

<label>姓名: <input type="text" id="username"></label>
<label>爱好: <input type="text" id="hobby"></label>
<label>年龄: <input type="text" id="age"></label>
<table border="1">
    <thead>
        <tr><th>姓名</th><th>爱好</th><th>年龄</th><th>操作</th></tr>
    </thead>
    <tbody>
        <tr><td>刘栋</td><td>美女</td><td>18</td><td><button class="del">删除</button></td></tr>
        <tr><td>王五</td><td>赵六</td><td>17</td><td><button class="del">删除</button></td> </tr> </tbody>
</table>


var btn= $('.add')
btn.click(function( ){
    var name= $('#usernam').val()
    var hobby= $('#hobby').val()
    var age= $('#age').val()
    var table=
            `<tr><td>${name}</td><td>${hobby}</td><td>${age}</td><td><button class="del">删除</button></td></tr>`
    $('tbody').append(table)
})
//删除  失败的写法 基于事件冒泡，事件委托必须依靠on进行绑定
$('.del').click(function( ){
    $(this).parent().parent().remove()
})
//事件委托的写法   上层标签对象.on('事件名称','委托人的选择器',function(){})
$('tbody').on('click','.del',function(){
    $(this).parent().parent().remove();
})
```

### 14.属性操作

```javascript
1.prop 针对的是 checked enable disable selected 属性
	- 查看 $().prop('checked')
	- 设置 $().prop('checked','bool')
2.attr
	- 查看 $().attr('checked')
	- 设置 $().attr(['xx':'00','hh':'gaga'])	
```

### 15.常用事件

```javascript
<style>
    .dd{background-color:red;height:200px;width:200px;}
.xdd{background-color:green;height:200px;width:200px;}
</style>

<input type="text" id="uname">
    <input type="text" id="xx">
    <select name="hobby" id="hobby">
        <option value="1">苍老师</option>
        <option value="2">小泽老师</option>
        <option value="3">柚子猫老师</option>
	</select>
<div class="dd"></div>
<input type="text" id="Inp">


1.focus 获取光标触发的事件
    $('#uname').focus(function( ){
    $(this).css({'background-color':'red'})
    })
2.blur 光标离开时触发的事件
    $('#uname').blur(function( ){
        $(this).css({'background-color':'yellow'})
    })
3.change 当域内的内容发生改变时触发的事件，input标签使用较少，一般用在select标签上
    $('#xx').change(function(){
        console.log($(this).val());
    })
    $('#hobby').change(function(){
        //获取所有option对象
        console.log(this.options);   
        //获取选中的option索引位置
        console.log(this.selectedIndex)  
        console.log(this.options[this.selectedIndex])
    })
4.鼠标悬浮事件
$('.dd').hover(
    //鼠标进入时触发的事件   添加类属性
    function(){
        $(this).addClass('xdd');		或toggleClass()
    },
    //鼠标离开时触发的事件   移除类属性
    function(){
        $(this).removeClass('xdd');		或toggleClass()
    }
)
```

### 16.页面加载

```javascript
<style>
    .c1{
        background-color:red;
        height:200px;
        width:200px;
    }


    .c2{
        background-color:blue;
        height:200px;
        width:200px;
    }
</style>
	
<div class="c1"></div>
<div class="c2"></div>
<img src="https://img02.sogoucdn.com/app/a/100520093/caeba5a347c20618-9f007285b539607f-8db5b27ed80f5e4e001b02f1a896fa2d.jpg">




1.最后执行，等待页面资源(html/css/视频/图片)加载完成后，存在覆盖现象，最后一个生效
    /*window.onload=function(){
        $('.c1').css({'background-color':'green'})
    }
    window.onload=function(){
        $('.c2').css({'background-color':'green'})
    }*/
2.jquery ready()不存在覆盖现象
$(document).ready(function(){
    $('.c1').click(function(){
        $(this).css({'background-color':'green'})
    })
})
$(document).ready(function(){
    $('.c2').click(function(){
        $(this).css({'background-color':'green'})
    })
})
```

### 其他：

```
bootstrap   和 fontawesome (饿了么开源的项目)
```



## Django

### 1.web框架初识

请求响应流程
![image-20231015131255101](assets/image-20231015131255101.png)

```javascript
djang flask tornado(异步) flaskApi....

浏览器先的到html框架，然后在向服务器请求js,css,picture等数据
请求的url是	协议+域名+路径   https://127.0.0.1:8000/hh.js
<script src='./hh.js'></script>

基于ip找到唯一的一台计算机(服务器)，基于port找到对应的应用程序。

```

```python
1.wsgi.py
	封装的socket模块
2.urls.py
	存储的是路由和视图函数的对应关系
3.views.py
	视图函数，处理业务逻辑
4.models.py
	和数据库建立联系
5.manage.py
	管理文件 
    sys.argv	执行py文件获取命令行的参数返回一个列表，第一个是py文件名,第二开始是参数。
6.templates文件夹
	存储的是html文件
7.static文件
	js、css、jpg...资源
```

**手撸web框架**

1. *阶段一*

```python
wigi.py文件下
import socket

socket_serve = socket.socket()
socket_serve.bind(('127.0.0.1', 8000))
socket_serve.listen()

while True:
    conn, address = socket_serve.accept()
    # 接受请求并打印
    request = conn.recv(1024)
    request = request.decode('utf-8')
    print(request)

    # 提取url路径
    path = request.split(' ')[1]
    print(path)
    # 返回给浏览器响应行
    conn.send(b'HTTP/1.1 200 ok\r\n\r\n')

    if path == '/home':
        with open('home.html', 'rb') as f:
            data = f.read()
        conn.send(data)
        # 断开连接
        conn.close()

    if path == '/index':
        with open('index.html', 'rb')as f:
            data = f.read()
        conn.send(data)
        conn.close()
    if path == '/login':
        with open('login.html', 'rb')as f:
            data = f.read()
        conn.send(data)
        conn.close()
```

```html
home.html文件下
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>home</title>
</head>
<body>
    <h1>home页面登录成功！</h1>
    <img src="https://img04.sogoucdn.com/app/a/100520093/f9d5c084396d06f6-0c7006bf1d0bb8d5-f6caaf58ef4225eb2307614676de3c85.jpg" alt="mn">
    <img src="http://img.mp.itc.cn/upload/20161228/0a37afba424f418f8f489b4ca4e9f394_th.jpg" alt="mn">
    <img src="https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2022%2F0703%2F288b0e1bj00reelni001sc000hs00hsg.jpg&thumbnail=660x2147483647&quality=80&type=jpg" alt="mn">
</body>
</html>


login.html文件下
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>login</title>
</head>
<body>
<h1>Login!登录成功</h1>
<img src="https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2022%2F0114%2Fa9e68e36j00r5oheq001fc000hs00qog.jpg&thumbnail=650x2147483647&quality=80&type=jpg" alt="hh">
</body>
</html>


index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
<h1>index!</h1>
<img src="https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2023%2F0510%2F12be05d7j00rug7vv003zd000xc01com.jpg&thumbnail=660x2147483647&quality=80&type=jpg">
<img src="https://nimg.ws.126.net/?url=http%3A%2F%2Fdingyue.ws.126.net%2F2023%2F0330%2Fd228cf4aj00rsbeex001jc000hs00uwg.jpg&thumbnail=660x2147483647&quality=80&type=jpg">
</body>
</html>
```

2. *阶段二 优化wsgi.py文件*

```python
#减少if判断
import socket

socket_serve = socket.socket()
socket_serve.bind(('127.0.0.1', 8000))
socket_serve.listen()
#绑定路径和资源对应关系
urlpatterns = [('/home', 'home.html'), ('/login', 'login.html'), ('/index', 'index.html')]
while True:
    conn, address = socket_serve.accept()
    # 接受请求并打印
    request = conn.recv(1024)
    request = request.decode('utf-8')
    print(request)

    # 提取url路径
    path = request.split(' ')[1]
    print(path)
    # 返回给浏览器响应行
    conn.send(b'HTTP/1.1 200 ok\r\n\r\n')
    for item in urlpatterns:
        if path == item[0]:
            with open(item[1], 'rb') as f:
                data = f.read()
            # 返回给浏览器数据，退出循环
            conn.send(data)
            conn.close()
            break;
```

3. *阶段三封装函数*

```python
import socket
import threading


def home(conn):
    with open('home.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()


def login(conn):
    with open('login.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()


def index(conn):
    with open('index.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()


socket_serve = socket.socket()
socket_serve.bind(('127.0.0.1', 8000))
socket_serve.listen()
urlpatterns = [('/home', home), ('/login', login), ('/index', index)]

while True:
    conn, address = socket_serve.accept()
    # 接受请求并打印
    request = conn.recv(1024)
    request = request.decode('utf-8')
    print(request)

    # 提取url路径
    path = request.split(' ')[1]
    print(path)
    # 返回给浏览器响应行
    conn.send(b'HTTP/1.1 200 ok\r\n\r\n')
    for item in urlpatterns:
        if path == item[0]:
            # 开启多线程
            t = threading.Thread(target=item[1], args=(conn,))
            t.start()
            # item[1](conn)

```

4. 阶段四 *连接数据库*

```python
import pymysql


def con_mysql():
    # 创建连接对象
    conn = pymysql.Connect(
        host='127.0.0.1',
        port='3306',
        user='root',
        password='678846',
        database='liu',
        charset='utf8'
    )
    # 创建游标对象
    cursor = conn.cursor()
    sql = 'show tables;'
    # 执行sql语句
    res = cursor.execute(sql)
    #提交事物
    conn.commit()
    conn.close()
```

5. 阶段五综合设置manage.py文件启动*

```python
wsgi.py
import socket
import threading
from urls import urlpatterns


def run():
    socket_serve = socket.socket()
    socket_serve.bind(('127.0.0.1', 8000))
    socket_serve.listen()

    while True:
        conn, address = socket_serve.accept()
        # 接受请求并打印
        request = conn.recv(1024)
        request = request.decode('utf-8')
        print(request)

        # 提取url路径
        path = request.split(' ')[1]
        print(path)
        # 返回给浏览器响应行
        conn.send(b'HTTP/1.1 200 ok\r\n\r\n')
        for item in urlpatterns:
            if path == item[0]:
                # 开启多线程
                t = threading.Thread(target=item[1], args=(conn,))
                t.start()
                # item[1](conn)

```

```python
urls.py
import views
urlpatterns = [('/home', views.home),
               ('/login', views.login),
               ('/index', views.index)
            ]
```

```python
models.py
import pymysql

def create_models():
    conn=pymysql.Connect(
        host='localhost',
        port=3306,
        user='root',
        password='',
        database='learning',
        charset='utf8'
    )
    cursor=conn.cursor()
    sql='create table useinfo ( id int primary key auto_increment,name char(10) not null,age int unsigned );'
    cursor.execute(sql)
    conn.commit()
    conn.close()
```

```python
views.py

def home(conn):
    with open('templates/home.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()


def login(conn):
    with open('templates/login.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()


def index(conn):
    with open('templates/index.html', 'rb') as f:
        data = f.read()
    conn.send(data)
    conn.close()

```

```python
manage.py
import sys
from wsgi import run
from models import create_models

# 执行py文件时，可以通过sys.argv返回一个列表 [py文件名,*args]
command = sys.argv
line = command[1]
if line == 'runserver':
    run()
elif line == 'migrate':
    create_models()
```

template文件下存放的是html模版

```html
homr.html
login.html
index.html
```

static 文件夹下存放的是静态资源 css js picture等

**特别浏览器会请求一个小图标默认路径是 href='favicon.icon'**

设置 <link  rel='icon' href='xx.icon'>

**步骤**

```
1.运行项目
2.接受请求，wsgi~模块
	- 接受请求，将请求信息封装成一个字典传递给application函数，并调用application函数。
	- application函数会根据路径找到对应的视图函数，并获取视图函数的返回值
	- application函数的返回值是一个列表，列表中封装的都是视图函数的返回值
	- wsgi~模块会将application的返回值通过socket传递给浏览器
```



### 2.Http协议和Django初始

```html
http : 超文本传输协议  超文本（带有链接的文本数据）
url 统一资源定位符		协议+域名+路径+？请求参数
请求和响应的步骤
	在浏览器输入url，按下回车后的步骤
	- 1.浏览器会向DNS服务器请求解析url中域名对应的ip地址
	- 2.浏览器根据IP地址和默认端口号80 找到服务器并建立连接
	- 3.浏览器发出读取文件的请求
	- 4.服务端做出响应，返回对应的html文件
	- 5.断开连接
	- 6.浏览器加载渲染html并显示内容

http 三次握手和四次挥手
```

 请求消息格式

![image-20231015220224444](assets/image-20231015220224444.png)

```python
	- 首行	method+url(路径)+https版本号
    - 请求头   多组参数(key-value)组成
    - 空行	/r/n/r/n  标志请求头结束
    - 请求体	  url请求参数 get为空  post参数全在body中
    
    get和post请求的区别
    	1.浏览器网址输入的都是get请求，form表单输入的内容是post请求
        2.get的参数在url中参数有长度限制，post的参数在请求体参数无长度限制
        3.post相对安全一点
```

响应消息格式

![image-20231016081937207](assets/image-20231016081937207.png)

network内容
```python
General部分

Request URL: http://127.0.0.1:8080/   请求地址
Request Method: GET   请求方法
Status Code: 200 OK   响应状态码和描述
Remote Address: 127.0.0.1:8080    客户端的地址(ip+port)
        
request headers 请求头部键值对信息
response headers  响应头部键值对信息
```

状态码
```python
1xx 	服务器收到请求
2xx 	请求成功
3xx 	重定向
4xx		客户端发生错误
5xx 	服务端发生错
```

MVC就是把Web应用分为模型(M)，控制器(C)和视图(V)三层
![image-20231016085839379](assets/image-20231016085839379.png)

django是MTV模式,其实就是从MVC模式加工过来的.
```python
M 代表模型（Model）： 负责业务对象和数据库的关系映射(ORM)。
T 代表模板 (Template)：负责如何把页面展示给用户(html)。  模板渲染功能
V 代表视图（View）：   负责业务逻辑，并在适当时候调用Model和Template。

+ url控制器   urls.py文件:  路径和视图函数的映射关系
```

django下setting.py
```python
#注册app
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01.apps.App01Config'
    # 'app01'
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 'DIRS': os.path.join(BASE_DIR , 'templates'),
        'DIRS': [BASE_DIR , 'templates'],  #注意这个配置
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
BASE_DIR = Path(__file__).resolve().parent.parent #BASE_DIR就是当前目录下
__file__:当前文件的绝对路径
os.path.dirname():找父级目录
```

案例用户登录：
```python
# 1 get请求回复登录页面
# 2 用户输入用户名和密码提交post请求到后台
# 3 后台将用户提交过来的数据取出,并判断用户名和密码是否正确,  username='root'  password='123'
# 4 用户名密码都是对的,回复一个新的html页面,  失败了,回复一个含有404字符串的页面

urls.py
    from django.urls import path
    from app01 import views
    urlpatterns = [
        path('home/', views.home),
    ]
    
views.py
	from django.http import HttpResponse
from django.shortcuts import render


def home(request):
    if request.method == 'GET':
        return render(request, 'home.html')
    elif request.method == 'POST':
        data = request.POST
        name = data.get('name')
        password = data.get('password')
        if name == 'root' and password == '123':
            return render(request, 'login.html')
        else:
            return HttpResponse('404')
templates文件下
	login.html	home.html
```



### 3.django的url

```python
虚拟环境：相互独立，相互隔离，同一个系统解释器，运行不同版本的包
#创建虚拟环境
virtualenv xx  --python python=3.8
#激活虚拟环境
cd ./xx/Scripts
activate
#退出虚拟环境
deactivate
```

```python
url注意的点	
    1.url(r'^index/', views.index),     路径的前置导航斜杠(对应根路径那个),不需要写,django自动加上
    2.http://127.0.0.1:8000/index 当我们访问django的url路径时,如果请求路径最后没有写/,那么django会发一个重定向的响应,告	  诉浏览器,加上/再来访问我
    settigns.py配置文件中修改:
        默认为True, 当值为True时,django需要请求路径后面加上斜杠,如果请求没有加,那么响应301重定向,让浏览器街上斜杠重新请			求
        APPEND_SLASH = True  
        值为False,就关闭了django的这个功能
        APPEND_SLASH = False        
	路由捕获成功了就不会继续循环了(for i in urlpatterns:...break)
    url(r'^index/$', views.index),  
    url(r'^index/xx/xx/', views.index2),  
    注意: 路径写正则时,要注意最好精确匹配,尾部加上$,或者写正则时,尽量不要路径差不多    

    url(r'^$', views.home),  #匹配根路径的写法  
```

```python
url的分组
	1.无名分组 (位置传参)
    	url(r'^books/(\d+)/(\d+)/', views.books) 
        视图函数写法:
        def books(request, y, m):
        print(y, m)  
        return HttpResponse('%s-%s所有书籍都在这儿,你随意看' % (y, m))    
    2.有名分组(函数参数必须和有名分组名称相同)
	url(r'^books/(?P<year>\d+)/(?P<month>\d+)/', views.books2)
    def books2(request,  month, year):
        print(year, month) 
        return HttpResponse('%s--%s所有书籍都在这儿,你随意看' % (year,month ))
```

```python
url的反向解析

```

```python
request对象
http://127.0.0.1:8000/home/12/123?a=1&b=2
request	<WSGIRequest: GET '/home/12/123?a=1&b=2'>
	1.method	GET
    2.POST
    3.GET	<QueryDict: {'a': ['1'], 'b': ['2']}>
    4.path 	/home/12/123
    5.get_full_path()	/home/12/123?a=1&b=2
    6.META	请求的所有信息
postman 发送post请求
	post 	<QueryDict: {}>
```

```python
响应方法
1.HttpResponse 	返回字符串
2.render	返回响应页面
3.redirect	重定向
4.JsonResponse	返回json格式字符串
```

```python
设置响应头和状态码
response=HttpResponse('hh')
设置响应头
response['name']='liudong'
设置状态码
response.status_code=404
```

```python
CBV和FBV
FBV:函数形式编写的视图
CBV:类形式编写的视图

urls.py
	urlpatterns = [
    path('home/', views.HomeView.as_view()),
]
    
views.py
	class HomeView(View):
    # 通过反射机制获取类中定义的请求方法
    def get(self, request, *args, **kwargs):
        print(request.GET)
        return HttpResponse('ok')

 base.py(源码部分)
	http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
	 def as_view(cls, **initkwargs):
        def view(request, *args, **kwargs):
            self = cls(**initkwargs)
            self.setup(request, *args, **kwargs)
            if not hasattr(self, 'request'):
                raise AttributeError(
                    "%s instance has no 'request' attribute. Did you override "
                    "setup() and forget to call super()?" % cls.__name__
                )
            return self.dispatch(request, *args, **kwargs)
        return view
    是一个闭包函数，返回了view函数，view函数又调用了dispatch()
    def dispatch(self, request, *args, **kwargs):
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
            return handler(request, *args, **kwargs)
	通过反射机制在定义类中找到对应的请求方法，没有就报错
反射机制：通过字符找到对应的属性和方法
```

```python
dispatch 用法和装饰器  在执行请求方法之前做一些操作

1.重写父类dispatch()
    def dispatch(self, request, *args, **kwargs):
        print('执行方法之前')
        ret = super().dispatch(request, *args, **kwargs)
        print('执行方法之后')
        return ret
2.写装饰器
	from django.utils.decorators import method_decorator
	def outer(func):
        def inner(*args, **kwargs):
            print('我是装饰器')
            ret = func(*args, **kwargs)
            print('我是装饰器')
            return ret
        return inner

	方式一：请求方法前加装饰器
		@method_decorator(outer)
        def get(self, request, *args, **kwargs):
            print(request.GET)
            return HttpResponse('ok')
	方式二：dispatch()方法前加装饰器
    	@method_decorator(outer)
        def dispatch(self, request, *args, **kwargs):
            ret = super().dispatch(request, *args, **kwargs)
            return ret
	方式三：类前面加装饰器
    	@method_decorator(outer,naem='get')
		class HomeView(View):
```

### 4.模版渲染

```python
views.py
    from django.shortcuts import render
	import datetime.datetime
    def home(request):
        1.字符串
        string = 'i love python'
        2.数字
        number = 123
        3.列表
        li = [1, 2, 3, 4]
        4.字典
        dic = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
	   5.函数   调用函数不用(),所以不能用有参数的函数
        def function():
            return '我是函数'
		
        class Class_name(object):
            def __init__(self):
                self.name = '类'
                self.data = '2023'
		6.类对象
        c = Class_name()
        #封装成字典
        date = datetime.now()
    	tag= '<a href="http://www.baidu.com">百度<a/>'
        data={
            'string':string,
            'number':number,
            'li':li,
            'dic':dic,
            'function':function,
            'c':c,
        }
        #模板渲染完之后，才返回给浏览器
        return render(request,'home.html',data)

    
home.html
	字符串{{ string }}
    数字{{ number }}
    <ul>
    列表{% for i in li %}
        <li>{{ i }}</li>
        {% endfor %}
    </ul>
    <ul>
    字典{% for k,v in dic.items %}
        <li>{{ k }}--{{ v }}</li>
        {% endfor %}
    </ul>
    <p>{{ function }}</p>
    <p>{{ c.name }}:{{ c.data }}</p>   
```

![image-20231018201150438](assets/image-20231018201150438.png)



```python
过滤器：对数据进行加工
语法：
	- 无参数过滤器 {{变量名|过滤器名称}}
    - 有参数过滤器 {{变量名|过滤器名称:'参数'}}	
    
    
django 自带过滤器
	1.length	返回长度
    2.default	为空或False设置默认值
    3.slice		切片
    4.date		格式化{{time|data:'Y-m-d H:i:s'}}
   	5.safe		xss攻击：跨站脚本攻击 标记为标签
    			a_tag = '<a href="http://www.baidu.com">百度<a/>'
				<h2>{{ a_tag|safe }}</h2>
   6.cut 		移除指定字符串
   7.join		拼接字符串
```

![image-20231018203717244](assets/image-20231018203717244.png)
![image-20231018203821541](assets/image-20231018203821541.png)

```python
1.forloop		对象：记录循环次数
{% for i in li %}
        <li>{{ forloop.counter }}---{{ i }}</li> 从1开始计数
        <li>{{ forloop.counter0 }}---{{ i }}</li> 从0开始计数
        <li>{{ forloop.revcounter }}---{{ i }}</li> 倒序,从1开始计数
        <li>{{ forloop.revcounter0 }}---{{ i }}</li> 倒序,从0开始计数
        <li>{{ forloop.first }}---{{ i }}</li> 如果是第一次循环,就得到True
        <li>{{ forloop.last }}---{{ i }} </li>  如果是最后一次循环,就得到True,其他为False
{% endfor %}

2.reversed	倒序循环

<ul>
    {% for i in hobby reversed %}
        <li>{{ i }} </li>
    {% endfor %}
</ul>

3.if 标签
{% if age > 18 %}
    <h1>太老了</h1>
{% elif age == 18 %}
    <h1>还行</h1>
{% else %}
    <h1>挺嫩</h1>
{% endif %}

{% if age > 18 or number > 100 %} <!-- 符号两边必须有空格 -->
    <h1>太老了</h1>
{% elif age == 18 %}
    <h1>还行</h1>
{% else %}
    <h1>挺嫩</h1>
{% endif %}

{% if hobby|length > 3 %} <!-- 可以配合过滤器来使用 -->
    <h1>爱好还挺多</h1>
{% else %}
    <h1>爱好不够多</h1>
{% endif %}

4.with	起别名
{% with string as s %}
        <li>{{ s}}</li>
    {% endwith %}
   

5.csrf_token验证
<form action="" method="post">
    {% csrf_token %}  <!-- 加上这个标签之后,post请求就能通过django的csrf认证机制,就不需要注释settings的配置了 -->
    <input type="text" name="uname">

    <input type="submit">
</form>

```

**Django模板中属性的优先级大于方法**

```python
d2 = {'items': [11,22,33]}
优先使用items属性,不使用items方法,容易导致错误

<ul>

    {% for key,v in d2.items %}
        <li>{{ key }} -- {{ v }}</li>
    {% endfor %}
</ul>
```

```python
模板继承
	1.先创建一个母版
    2.在母板中预留block块(钩子)
    	{% block body %}
       	 <h1>母板</h1>
    	{% endblock %}
    3.继承母板
    	{%extends 'path'%}
    4.重写母板并继承中block块
    	{% block body %}
        {{block.super}}
            <...>
        {% endblock%}
     
   其他位置的block块
	{%block css%}
    {%endblock%}
    {%block js%}
    {%endblock%}
```

 

```python
组件：一个完整功能的模块，其他页面要使用，以组件形式引入
{% include 'zujian.html' %}
```



```python
静态文件配置流程：
	1.setting.py
		#别名，映射到静态文件的路径，引入时使用别名
		STATIC_URL = '/static/'
        #静态文件的路径，修改不会影响别名的使用
		STATICFILES_DIRS = [
    	os.path.join(BASE_DIR, 'statics'),
				]
	2.在项目目录下创建一个文件夹statics
    3.在html引入
    	引入方式一：
        	<link rel="stylesheet" href="/static/css/xx.css">  使用别名路径引入
         引入方式二：
        	{%load static %}
            <link rel="stylesheet" href="{%static 'css/xx.css' %}"> 
```



```python
自定义过滤器
1.在应用文件夹下创建一个templatetags文件夹
2.templatetags文件下创建py文件
3.在py文件下编写
my_tag.py文件下
	from django import template
    # 定义一个注册器
    register = template.Library()
    # 自定义过滤器最多有俩个参数，第一个参数是管道前的数
    @register.filter
    def together(x, y):
        return x + y
home.html文件下
	{{ string|together:' liud' }}
    
    
自定义标签
my_tag.py
	# 自定标签，没有参数限制
    @register.simple_tag
    def tag(x1, y1, x2, y2):
        return x1 + y1 + x2 + y2
home.html
	<p>{% tag 'liu' 'dong' '18' 'h'%}</p>
    
    
自定义动态组件
数据的流向：视图函数-->html模版-->inclusion_tag函数-->小段html-->html模版
my_tag.py
	# 动态组件 需要传入一个参数,这个参数就是一个html文件(你想做成动态组件的html文件)
    @register.inclusion_tag('zujian.html')
    def zujian(xx):  # 参数没有个数限制
        data = xx
        return {'data': data}
    # zujian.html会收到定义的inclusion_tag函数的返回值,然后进行zujian2.html这个动态组件的模板渲染
home.html
	{% zujian li%}
zujian.html
	<ul>
        {% for item in data %}
        <li>{{ item }}</li>
        {% endfor %}
    </ul>
```

![image-20231019193515951](assets/image-20231019193515951.png)




### 5.orm

```python
orm:关系对象模型 
orm->sql->pymysql->mysqld->磁盘
类-表 对象-数据
创建模型类
    # 属性对应的字段,默认都是不能为空的,也就是加了not null约束
    class Book(models.Model):

        # 如果没有指定主键字段,默认orm会给这个表添加一个名称为id的主键自增字段
        # 如果制定了,以指定的为准,那么orm不在创建那个id字段了
        # nid = models.AutoField(primary_key=True)  #int primary_key auto_increment,
        title = models.CharField(max_length=32)  #varchar 书籍名称
        # price = models.FloatField()  #
        price = models.DecimalField(max_digits=5, decimal_places=2)  # 999.99 价格
        pub_date = models.DateField()  # date  出版日期
        publish = models.CharField(max_length=32)  #出版社名称
        # xx = models.CharField(max_length=18, null=True, blank=True)  # null=True,blank=True允许该字段数据为空
        # xx = models.CharField(max_length=18, default='xxx')  # null=True,blank=True允许该字段数据为空

    Book生成的表名称为 应用名称_模型类名小写

数据库同步指令
	python manage.py makemigrations
	python manage.py migrate
生成的有个django_migrations表记录的是migrations文件夹下那些文件被执行了，执行migrate会先检测这张表那些文件执行了，如果执行了就不在执行了。
```



```python
django配置连接mysql
1.创建数据库
	create database orm1 charset utf8;
2.setting.py配置
	DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'learning',
        'HOST':'127.0.0.1',
        'PORT':3306,
        'USER':'root',
        'PASSWORD':'',  #有密码的填密码
        
        }
    }
3.安装pymysql
	pip install pymysql -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
4.在项目主目录下
	import pymysql
    pymysql.install_as_MySQLdb()
5.执行数据库同步指令
	只要换了数据库就会在执行一边(默认db.sqlite3)
```

![image-20231019203249868](assets/image-20231019203249868.png)




```python
记录的增删改查

增加
	1.实例化模型类
    	from app01 import models
        from django.shortcuts import HttpResponse
        def home(request):
            new_book = models.Book(
                title='python入门到放弃',
                price=20,
                # datetime.datetime.now().strftime("%Y-%m-%d_%H:%M:%S")
                pub_date='2023-10-10',
                publish='夕阳红出版社'
            )
            new_book.save()

            return HttpResponse('ok')
        主键，有默认值或者可以为空的，可以不用传值
    2.create()方法，返回的是新添加的模型类对象
    	models.Book.objects.create(
        title='Python面向监狱编程',
        price=20,
        pub_date='2023-10-10',
        publish='洛杉矶出版社'
    	)
    3.批量添加 bulk_create()
        obj_list=[]
        for i in range(1,10):
            book_obj=models.Book(
                title=f'白鹿原{i}',
                price=20,
                pub_date=f'2023-10-{i}',
                publish='家里蹲出版社'
            )
            obj_list.append(book_obj)
        models.Book.objects.bulk_create(obj_list)

```

![image-20231019214807638](assets/image-20231019214807638.png)



```python
查询
book_objs = models.Book.objects.all()  # queryset 类似于列表
print(book_objs)
book_objs = models.Book.objects.filter(id=3)  # 条件查询 结果为queryset类型数据
print(book_objs)
book_objs=models.Book.objects.filter()  # filter没有加条件,和all一样的效果
print(book_objs)
book_objs=models.Book.objects.filter(id=100)  # 查不到数据,不会保存,返回空干的queryset类型数据
print(book_objs)
book_objs = models.Book.objects.get(id=3)  # 条件查找 结果为: 模型类对象
print(book_objs)
# models.Book.objects.get()  # 也是查所有但是get方法的查询结果有要求, 有且只能有一条
# models.Book.objects.get(id=100)  # 查询不到，查询多条都会报错
```

![image-20231019215349656](assets/image-20231019215349656.png)



```python
删除
# queryset类型数据可以调用delete方法删除查询结果数据
models.Book.objects.filter(id=3).delete()
# 模型类对象也可以调用delete方法删除数据
models.Book.objects.get(id=4).delete()
```

![image-20231019215700102](assets/image-20231019215700102.png)


```python
修改
	# 修改方式1  通过queryset类型数据修改
    models.Book.objects.filter(id=6).update(
        price=20,
        title='红浪漫',
    )
    # 报错:模型类对象不能调用update方法
    # models.Book.objects.get(id=5).update(
    #     price=30,
    # )
    # 修改方式2  通过模型类对象来修改
    ret = models.Book.objects.get(id=5)
    ret.price = 30
    ret.title = '少年阿宾00'
    ret.save()
```

![image-20231019220117787](assets/image-20231019220117787.png)






































​		
