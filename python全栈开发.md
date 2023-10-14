## JavaScript

### 1. js的四种引入方式

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

## Bootstrap

### 













































































