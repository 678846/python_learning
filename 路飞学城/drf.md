

## 第八节 面对对象编程

1. 对象是存放数据和功能的容器

2. 类是用来存放多个对象相同的数据和功能的容器

   ```
   优点：同一种类对象把相同的数据和功能放到类中，每个对象无需重复存一份，每个对象只需存储独特的数据即可。
   ```

3. 对象和类的关系： 对象是类的实例化，类是对象的抽象

4. 

```

```



## python回收机制

```
引用计数器为主，标记清除和分代回收为辅 + 缓存机制
在python中维护了一个环形双向链表，链表中存储的都是python程序创建的所以对象，每个对象都有一个引用计数器，如果引用计数器的值为0时，就会垃圾回收（将对象从链表中移除，销毁对象释放内存）但是由多个元素组成的对象，可能存在循环引用的问题。python又引入了标记清除和分代回收，在其内部为4个链表，在内部达到各自的阈值时，会触发扫描链表标清除的动作。
- 缓存机制  
	池：避免重复创建和销毁常用对象。
	free_list:如果引用计数器值为0时，不会直接进行回收，而是放到free_list当做缓存。
```



***



## django_rest_framework-note(武沛齐)

- http和https的区别
  - http是超文本传输协议，无状态的短连接（一次请求，一次响应，断开连接）信息是明文传输的。
  - https是具有安全性的加密传输，一般需要CA证书，因此需要费用。
- request和post请求的区别

  - request的参数长度是有限的，一般在url上
  - post参数在请求体上，没有长度限制
- 什么是前后端分离？

  - 前端提供 前端代码（html+css+js）

  - 后端  api接口   （json数据）
- 什么是drf，作用是什么？

  - djangorestframework

  - 更方便的提供api接口
- postman
  - 测试 -模拟浏览器发送请求
- super() 根据mro的继承顺序找他的父类
- csrf_exempt  如果是post请求，免除csrf_token验证

  - django 中的request 是请求相关的所有数据
  - request.method   body  GET POST FILES
- drf  中reuqest的封装了其他数据      
  -  request__request.GET ...  
  - url请求传递的参数可以通过self.args ,self.kwargs 获取

- FBV和CBV

  ```
  FBV:基于函数编写的视图
  CBV：基于类编写的视图   调用方法 as_view() 本质是view()
  ```

1. 前后端分离

   ```
   分离：用户系统&专业人士
   不分离：后台系统&用户量少
   ```

2. drf 配置  

   ```
   匿名用户的配置 request.user   request.auth
   ```

3. request对象

   ```python
   - 面向对象基础
   	class  Person():
           def __init__(self,name,age):
   			self.name=name
               self.age=age
           def show():
               return f'{self.name}{self.age}'
           #当访问不存在的成员时，就会触发
           def __getattr__(self,item):
               print(item)
           #无论成员是否存在都会触发
           def __getattribute__(self,item):
               return 123
       obj=Person('刘栋',18)
       print(obj.name)
       print(obj.age)
       print(obj.show())
       print(obj.xxx)
       - 反射机制：根据字符串去已经存在模块找指定的属性  getatrr(obj,'show')
       object的getattribute处理机制“
       	- 对象中有值，就返回
           - 对象中无值，就报错
   ```

   ```python
   -源码分析
   class HttpResponse():
       def __init__(self):
           pass
       def method(self):
           return 'v1'
       def use_info(self):
           return 'v2'
   class DrfResponse():
       def __init__(self,request,n1):
           self._request=request
           self.n1=n1
        def __getattr__(self,attr):
           try:
               return getattr('self._request',attr)  #self._request.attr
           except AttributeError:
               return self.__getattribute__(atrr)
   request=HttpResponse()
   request=DrfResponse(requests,xx)
   
   
   request.method() 
   request.GET   #request._request.GET
   
   ```

4. 认证组件
   ```python
   	- 100个api，一个无需登录访问，99个都需要登录访问
   	- 实现
   		- 编写类(继承) ->认证组件
   			1. 获取请求的token
   			2. 校验token
   			3. 返回值
   				- 认证成功 返回元祖(request.user,request.auth)
   				- 认证失败 抛出异常
   				- 多个认证类[...]  返回None
   		- 应用组件
   			- 局部配置 AUTHENTICATION_CLASSES=[....]
   			- 全局配置 
   				REST_FRAMEWORK={'DEFAULT_AUTHENTICATION_CLASSES',['文件名.模块名.类名']}
   				查找顺序：先加载全局的，后加载局部的
   		- 状态码一致性
   			- 认证类中编写 authenticate_headers(self,request) 函数
   
   ```

- **注意：认证组件不能写在视图函数中,会存在循环引用的问题**

- 面向对象-继承
  ```python
  
  class Base():
      xxx=123
      def __init__(self):
      	pass
      def f1(self):
          self.f2()
       def f2(self):
          print("base.f2()")
  class Foo(Base):
      xxx=999
      def f2(self):
          print("foo.f2()")
  obj=Foo()
  obj.f1()		#foo.f2()
  obj.xxx  		#999
  ```

- 认证组件源码  加载认证组件,实例化每个认证类，封装到request对象中

- **python开发扩展**

```python
class Foo():
    #对子类进行约束，子类中必须定义这个方法
    def f1(self):
        raise NotImplemented(' ..... ')   
   
```

5. 案列 用户登录+认证
   ``` 
   POST  json -> name  password
   返回一个token
   
   
   GET 头或URL—> 携带token   
   	- request.query_params   #URL参数
   	- request.META        	 #请求头部信息
   ```


小结：

```
认证组件=[认证类1，认证类2，认证类3....]   执行每个认证类的authenticate()方法 
										- 认证成功或失败，不会向后继续执行
										- 返回None 继续向后执行
```

6. 权限组件 
   应用  全局和局部permission_classes=[类名,]

   ```
   权限组件=[权限类,权限类,权限类,......]   ->执行每个权限类的has_permission()方法
   										- 执行所有权限类
   										- 默认保证has_permission()方法全部返回True
   ```

   ***

   ```
   权限组件=[权限类,权限类,权限类,......]   ->执行每个权限类的has_permission()方法
   										- 执行所有权限类
   									   学会源码流程：扩展+自定义  -->修改check_permission()方法
   - has_permission() 中message=[{}]  ->无权限，返回的信息
   ```

   ***

     *认证类之间是or关系 ，权限类之间是and关系*

7. 思考题

   ```
   - response对象不好用 怎么替换   dispatch
   - drf中的组件和django的中间件的关系？  中间件->组件
   	as_view()->view()免除了csrf验证
   	1. 创建了CBV视图函数对象
   	2. 执行对象dispatch
   		2.1 封装请求对象
   		2.2 认证
   		2.3 权限处理
   ```

8. 限流组件

   ```python
   开发过程中，mo个接口不想被用户多度访问，限流机制  - eg 平台1个小时只能发10次  ip限制  验证码
   限制访问频率:
   	- 已登录的用户:用户主键-->id 用户名
   	- 未登录的用户: ip限制
   编写类
       from rest_framework.throttling import SimpleRateThrottle
       from django.core.cache import cache as default_cache
   
   	- get_cache_key() #凭借用户名+ip 返回一个字符串  scope ident
      class MyThrottle(SimpleRateThrottle):
       scope = 'xxx'
       # 访问频率   每分钟5次
       THROTTLE_RATES = ['XXX', '5/m']
       cache = default_cache
   
       def get_cache_key(self, request, view):
   
           if request.user:
               ident = request.user  # 用户ID
           else:
               ident = self.get_ident()  # 获取 IP
           return self.cache_format % {"scope": self.scope, "ident": ident}
   
   应用类
   	- 局部配置 throttle_classes = [MyThrottle]
   settings.py 配置
   	CACHES = {
       "default": {
           "BACKEND": "django_redis.cache.RedisCache",
           "LOCATION": "redis:127.0.0.1:6379",     # 集群换行即可
           "OPTIONS": {
               "CLIENT_CLASS": "django_redis.client.DefaultClient",
               "CONNECTION_POOL_KWARGS": {"max_connections": 200},
               # "PASSWORD": "密码",
   
           }
       },
   }
   ```

   































