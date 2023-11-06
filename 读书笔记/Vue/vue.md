

# Vue



## 1. Vue初识

### 1.1 什么是Vue

是一套构建用户界面的渐进式框架，采用MVVM设计模式，支持数据驱动和组件化开发
渐进式：框架的出现主要是为了解决单页应用程序（SPA）中的一些问题，例如在首次加载时加载过多的代码和资源，导致应用程序加载时间过长。渐进式框架通过将应用程序拆分成更小的块，可以更快地将应用程序呈现给用户。
单页面应用：在切换页面时，不会刷新整个页面，而是通过Ajax异步加载获取数据，改变页面内容

### 1.2MVVM设计模式

View：指的是视图部分，即DOM元素，负责视图的处理。
Model：指的是数据部分，负责业务数据
ViewModel：连接视图和数据的视图模型，负责监听Model和View的修改

在MVVM中，视图和数据不能直接通信，视图模型就相当于一个观察者，监控着双方的动作，并及时通知进行响应的操作

### 1.3 Vue的优势

1. 响应式系统：Vue可以自动追踪依赖关系并在依赖项改变时自动更新DOM，开发者只需要关注核心业务逻辑，而不需要手动操作DOM。
2. 组件化开发：Vue采用组件化开发方式，使得代码更加模块化、复用性更好，易于维护和测试。
3. 轻量级框架：Vue是一个轻量级的框架，体积小、速度快，可以与其他库或已有项目无缝连接。
4. 强大的指令系统：Vue拥有强大的指令系统，通过简单的标记语法，将模板渲染为DOM。
5. 模板语法简单易懂：Vue的模板语法简洁明了，易于学习，即使是前端新手也能快速上手。
6. 插件化扩展：Vue是一个非常灵活的框架，开发者可以根据自己的需求选择相应的插件进行扩展，从而加强Vue的功能。

## 2. Vue开发基础

### 2.1 Vue实例

#### 2.1.1 创建实例对象

```js
    var vm = new Vue({
        //选项
    })
```

| 选项       | 说明            |
| :--------- | --------------- |
| data       | Vue实例数据对象 |
| methods    | Vue实例的方法   |
| components | 定义子组件      |
| computed   | 计算属性        |
| filters    | 过滤器          |
| el         | 唯一根标签      |
| watch      | 监听数据变化    |



#### 2.1.2 el唯一根标签

```js
<body>
<div id="app">
    {{ name }}
</div>
</body>
<script>
    var vm = new Vue({
        //设置当前vue对象控制标签范围
        el:'#app',
        data: {
            name: 'vue实例创建成功！'
        }
    })
</script>
```

![image-20231105194325208](assets/image-20231105194325208.png)


#### 2.1.3 data初始数据

```js
<body>
<div id="app">
    {{ name }}
</div>
</body>
<script>
    var vm = new Vue({
        //设置当前vue对象控制标签范围
        el:'#app',
        data: {
            name: '定义初始数据！'
        }
    })
    console.log(vm.$data.name)
    console.log(vm.name)
</script>
```

![image-20231105194557646](assets/image-20231105194557646.png)


#### 2.1.4 mothods 定义方法

*methods定义的方法，this指向当前vue对象*

```js
<body>
<div id="app">
<!--    绑定click事件-->
    <button @click="show_info">点击</button>
    <p>{{ msg }}</p>
</div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data: {
            msg: ''
        },
         methods:{
            //设置事件处理方法
             show_info(){
                 this.msg='触发点击事件'
             }
    }
    })
</script>
```

![image-20231105195226192](assets/image-20231105195226192.png)![image-20231105195239963](assets/image-20231105195239963.png)


#### 2.1.5 computed 计算属性

```js
<body>
<div id="app">
    <p>总价格：{{total_price}}</p>
    <p>单价：{{price}}</p>
    <p>数量：{{num}}</p>
    <button @click="num==0?0:num--">减少数量</button>
    <button @click="num++">增加数量</button>
</div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data: {
            price:20,
            num:0
        },
        computed:{
            //总价格 totall_price
            total_price(){
                return this.num*this.price
            }
        }
    })
</script>
```

![image-20231105200054537](assets/image-20231105200054537.png)


#### 2.1.6 watch监听状态

*监听当前vue实例中的数据变化，就会调用当前数据所绑定的事件处理方法*

```js
<body>
<div id="app">
    <p>{{num}}</p>
</div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data: {
            num:20
        },
        //使用watch监听num数据变化
        watch:{
     		//接受num
            num(res){
                console.log('num改变了',res);
            }
        }
    })
</script>
```

![image-20231105201245173](assets/image-20231105201245173.png)


#### 2.1.7 filters过滤器

```js
<body>
<div id="app">
    <p>{{msg|upper}}</p>
</div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data: {
            msg:'hello,world!'
        },
        filters:{
            //将hello,world 变为大写
            upper(value){
                return value.toUpperCase()
            }
        }
    })
</script>
```

![image-20231105202331528](assets/image-20231105202331528.png)


### 2.2 Vue数据绑定

#### 2.2.1 绑定样式

1. 绑定内联样式
   ```js
   <body>
   <div id="app">
       <div v-bind:style="{backgroundColor:pink,width:width,height:height}">
           <div v-bind:style="my_div"></div>
       </div>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               my_div: {backgroundColor: 'red', width: '100px', height: '100px'},
               pink: 'pink',
               width: '100%',
               height: '200px',
           },
       })
   </script>
   ```

   ![image-20231105203906895](assets/image-20231105203906895.png)
   
2. 绑定样式类
   ```js
   <style>
       .box{
           background-color: pink;
           width: 100%;
           height:200px;
       }
       .inner{
           background-color: red;
           width: 100%;
           height: 50px;
           border:2px solid solid white
       }
   </style>
   
   <body>
   <div id="app">
       <div v-bind:class="box">
           <div v-bind:class="inner">inner1</div>
           <div v-bind:class="inner">inner2</div>
       </div>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               box:'box',
               inner:'inner',
           },
       })
   </script>
   ```

   ![image-20231105204455917](assets/image-20231105204455917.png)
   

#### 2.2.2内置指令

1. v-model 双向数据绑定
   ```js
   <body>
   <div id="app">
      <input type="text" v-model="msg">
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               msg:'v-model指令',
           },
       })
   </script>
   ```

   ![image-20231105205400613](assets/image-20231105205400613.png)
   

   ![image-20231105205308583](assets/image-20231105205308583.png)
   
2. v-text 元素内部插入文本内容
   ```js
   <body>
   <div id="app">
       <p v-text="msg"></p>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               msg:'我是v-text插入的内容',
           },
       })
   </script>
   ```

   ![image-20231105205736622](assets/image-20231105205736622.png)
   
3. v-html 元素内部插入html标签内容
   ```js
   <body>
   <div id="app">
       <div v-html="msg"></div>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               msg:'<p>我是v-html</p>',
           },
       })
   </script>
   ```

   ![image-20231105205924840](assets/image-20231105205924840.png)
   
4. v-for 列表渲染
   ```js
   <body>
   <div id="app">
       <ul  v-for="(value,index) in list">
           <li>{{value}}--{{index}}</li>
       </ul>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               list:['python','Java','c++'],
           },
       })
   </script>
   ```

   ![image-20231105211427585](assets/image-20231105211427585.png)
   
5. v-bind 单向数据绑定
   ```js
   <body>
   <div id="app">
       <input v-bind:value="msg1">
   <!--    简写-->
       <input :value="msg2">
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data:{
               msg1: '我是v-bind',
               msg2: '我是v-bind',
           }
       })
   </script>
   ```

   ![image-20231105212036550](assets/image-20231105212036550.png)
   
6. v-on 事件监听指令，与事件类型配合
   ```js
   <body>
   <div id="app">
       <P>{{msg}}</P>
       <button @click="show">点击看看</button>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data:{
               msg: '请单击查看内容',
           },
           methods:{
               //methods 中定义的方法this都指向当前vue对象
               show(){
                   this.msg='哈哈，我是v-on指令'
               }
           }
       })
   </script>
   ```

   ![image-20231105212824672](assets/image-20231105212824672.png)
   
7. v-if 和 v-show 控制元素的显示和隐藏

   *v-if 会对元素进行删除和重新创建*
   *v-show 是操作元素的display属性*

   ```js
   <body>
   <div id="app">
       <div v-if="isshow" style="background-color:tan">我是v-if</div>
   <!--    每次点击isshow属性都会取反值-->
       <button @click="isshow=!isshow">显示或隐藏</button>
   </div>
   </body>
   <script>
       var vm = new Vue({
           el: '#app',
           data: {
               isshow: true,
           },
       })
   </script>
   ```

   ![image-20231105213757672](assets/image-20231105213757672.png)
   *v-if 点击之后会删除标签*

   把v-if 改成v-show 点击之后只是修改了标签的display属性

   ![image-20231105214002812](assets/image-20231105214002812.png)
   

## 2.2.3 学生列表案例

```js
<body>
<div id="app">
    <button @click="add_student">添加学生</button>
    <button @click="delete_student">删除学生</button>
    <table border="1" width="50%" style="border-collapse: collapse">
        <thead>
        <tr>
            <th>班级</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
        </tr>
        </thead>
        <tbody>
        <tr align="center" v-for="item in student_list">
            <td>{{item.grade}}</td>
            <td>{{item.name}}</td>
            <td>{{item.gender}}</td>
            <td>{{item.age}}</td>
        </tr>
        </tbody>
    </table>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            student_list: [
                {grade: '1', name: '张三', gender: '男', age: '18'},
                {grade: '2', name: '李四', gender: '女', age: '20'},
                {grade: '3', name: '王五', gender: '男', age: '22'}
            ],
        },
        methods: {
            //添加学生
            add_student() {
                var student = {grade: '3', name: '土豆', gender: '男', age: '12'}
                //将变量添加到列表
                this.student_list.push(student)
            },
            //删除学生
            delete_student() {
                this.student_list.pop()
            }
        }
    })
</script>
```

![image-20231105220832507](assets/image-20231105220832507.png)


## 2.3 Vue事件

**常用事件修饰符**

| 修饰符   | 说明                         |
| -------- | ---------------------------- |
| .stop    | 阻止冒泡事件                 |
| .prevent | 阻止默认事件                 |
| .capture | 事件捕获                     |
| .self    | 绑定到自身，只有自身才能触发 |
| .once    | 事件只触发一次               |







































