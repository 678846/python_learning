# DRF



## 1. CBV的简单使用

views.py
```python
from django.http import HttpResponse
from django.views import View


class BookView(View):
    def get(self, request):
        return HttpResponse('GET View')

    def post(self, request):
        return HttpResponse('POST View')

    def delete(self, request):
        return HttpResponse('DELETE View')
    
url：http://127.0.0.1:8000/book/
```

注意：如果访问的是http://127.0.0.1:8000/book 会重定向到http://127.0.0.1:8000/book/ （get请求）
![image-20231231191230708](assets/image-20231231191230708.png)



- 面向对象反射机制
  ***利用字符串到已存在的模块找到指定的属性和方法***

  ```python
  class Fruit:
      # 构造方法
      def __init__(self,name,color):
          self.name = name
          self.color = color
      # 类的普通方法
      def buy(self,price,num):
          print("水果的价格是：",price*num)
  """
      hasattr(object,'attrName'):判断该对象是否有指定名字的属性或方法，返回值是bool类型
      setattr(object,'attrName',value):给指定的对象添加属性以及属性值
      getattr(object,'attrName'):获取对象指定名称的属性或方法，返回值是str类型
      delattr(object,'attrName'):删除对象指定名称的属性或方法值，无返回值
  """
  apple = Fruit("苹果","红色")
  print(hasattr(apple,'name')) # 判断对象是否有该属性或方法
  print(hasattr(apple,'buy'))
  
  # 获取对象指定的属性值
  print(getattr(apple,'name'))
  print(apple.name)
  
  f = getattr(apple,'buy')
  f(5,10)
  # 设置对象对应的属性
  setattr(apple,'weight',100)
  
  # 删除对象对应的属性
  delattr(apple,'name')
  print(hasattr(apple,'name'))
  ```

  ![image-20231231194110140](assets/image-20231231194110140.png)
  
- 源码大致流程
  ```python
  class View:
  
      http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
  
      def __init__(self, **kwargs):
          for key, value in kwargs.items():
              setattr(self, key, value)
  
      @classonlymethod
      def as_view(cls, **initkwargs):
          def view(request, *args, **kwargs):
              self = cls(**initkwargs)	# cls BookView类对象
              return self.dispatch(request, *args, **kwargs)
          return view
  
      def dispatch(self, request, *args, **kwargs):
          if request.method.lower() in self.http_method_names:
              # self 实例化类对象
              handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
          return handler(request, *args, **kwargs)
  
      
  class BookView(View):
      def get(self, request):
          return HttpResponse('GET View')
  
      def post(self, request):
          return HttpResponse('POST View')
  
      def delete(self, request):
          return HttpResponse('DELETE View')
      
  
      
  BookView.as_view() --> view(客户端访问时执行) --> dispatch() --> handler()
  ```

  

## 2. 序列化器

### 2.1 restful 接口

```python
/book/  GET   	获取所有资源，返回所有资源
/book/  POST   添加资源，返回资源
/book/id  GET   获取某个资源，返回某个资源
/book/id  PUT   编辑某个资源，返回编辑之后的资源
/book/id  DELETE   删除某个资源，返回空
```

### 2.2 简单使用

```python
from rest_framework import serializers
from rest_framework.response import Response
from rest_framework.views import APIView
from sers.models import Book

# Create your views here.


# 针对模型设计序列化器
class BookSerializers(serializers.Serializer):
    title = serializers.CharField(max_length=32)
    price = serializers.IntegerField()
    pu_date = serializers.DateField()

# APIView 对request进一步封装，有 认证 权限 限流 三大组件
class BookView(APIView):
    def get(self, request, *args, **kwargs):
        # 获取所有数据
        book_obj = Book.objects.all()
        # 构造序列化器对象，序列化
        serializer = BookSerializers(instance=book_obj, many=True)  # 多个模型类对象
		
        '''
        tmp=[]
        for book in book_obj:
            d={}
            d['title'] = book.title
            d['price'] = book.price
            d['pu_date'] = book.pu_date
            tmp.append(d)
        '''
        
        # 返回json数据
        return Response(serializer.data)

    def post(self, request):
        # 添加一条数据
        print("Request POST", request.data)
        # 构造序列化器对象，反序列化
        serializer = BookSerializers(data=request.data)
        # 校验数据  成功 serializers.validated_data   失败 serializer.errors
        if serializer.is_valid():  # 全部校验成功返回True
            # 保存到数据库
            Book.objects.create(**serializer.data)

            return Response(serializer.data)
        else:
            return Response(serializer.errors)
```

![image-20240101200605405](assets/image-20240101200605405.png)
![image-20240101200626861](assets/image-20240101200626861.png)

### 2.3 配合前端修改相关参数 source

```python
class BookSerializers(serializers.Serializer):
    title = serializers.CharField(max_length=32)
    price = serializers.IntegerField()
    date = serializers.DateField(source="pu_date")   #book.pu_date

    '''
        tmp=[]
        for book in book_obj:
            d={}
            d['title'] = book.title
            d['price'] = book.price
            d['date'] = book.pu_date
            tmp.append(d)
    ''' 
```

![image-20240101201052999](assets/image-20240101201052999.png)


### 2.4 序列化器的save()

```python
# 针对模型设计序列化器
class BookSerializers(serializers.Serializer):
    title = serializers.CharField(max_length=32)
    price = serializers.IntegerField()
    date = serializers.DateField(source="pu_date")

    def create(self, validated_data):
        new_obj = Book.objects.create(**self.validated_data)
        return new_obj

class BookView(APIView):  
    def post(self, request):
        # 添加一条数据
        print("Request POST", request.data)
        # 构造序列化器对象
        serializer = BookSerializers(data=request.data)
        # 校验数据  成功 serializers.validated_data   失败 serializer.errors
        if serializer.is_valid():  # 全部校验成功返回True
            # 保存到数据库
            serializer.save()   # 只有data，会调用create()方法需要自定义
            return Response(serializer.data)
        else:
            return Response(serializer.errors)
```

![image-20240102112721051](assets/image-20240102112721051.png)


### 2.5 补充 (put,delete,get)

```python
class BookSerializers(serializers.Serializer):
    title = serializers.CharField(max_length=32)
    price = serializers.IntegerField()
    date = serializers.DateField(source="pu_date")

    def create(self, validated_data):
        new_obj = Book.objects.create(**self.validated_data)
        return new_obj

    def update(self, instance, validated_data):
        Book.objects.filter(id=instance.id).update(**self.validated_data)
        updated_obj = Book.objects.get(id=instance.id)
        return updated_obj


class BookDetailView(APIView):
    def get(self, request, id):
        obj = Book.objects.get(id=id)
        # 构造序列化对象
        serializer = BookSerializers(instance=obj)
        return Response(serializer.data)

    def put(self, request, id):
        # 构造序列化器
        book_obj = Book.objects.get(id=id)
        serializer = BookSerializers(data=request.data, instance=book_obj)

        if serializer.is_valid():  # 校验数据
            # 保存到数据库
            serializer.save()  # instance,data 调用update()方法
            return Response(serializer.data)

    def delete(self, request, id):
        Book.objects.get(id=id).delete()
        return Response()
```

![image-20240102161208676](assets/image-20240102161208676.png)
![image-20240102161227155](assets/image-20240102161227155.png)

![image-20240102161257255](assets/image-20240102161257255.png)


### 2.6 ModelSerializer

```python
class BookSerializers(serializers.ModelSerializer):
    # 自定义字段
    date = serializers.DateField(source='pu_date')

    class Meta:
        # 指定关联的模型类
        model = Book
        # fields='__all__'  # 所所有字段
        exclude = ['id', 'pu_date']  # 排除字段 和 fields 不能一起使用
 

定义了create和update方法
```





## 3. 视图

### 3.1 APIView

APIViews的特点：
1）集成自View 　　　　　　　　　　2）提供了自己的request对象
3）提供了自己的response对象　　　　4）提供了认证，权限，限流等功能



### 3.2 GenericAPIView

1. get_queryset() 获取全局queryset变量
2. get_serializer_class() 获取全局的序列化器
3. get_serializer() 获取全局的序列化器对象
4. get_object() 获取模型对象，设置有名分组PK

```python
from rest_framework.generics import GenericAPIView
from rest_framework import serializers

class BookSerializers(serializers.ModelSerializer):
    # 自定义字段
    date = serializers.DateField(source='pu_date')

    class Meta:
        # 指定关联的模型类
        model = Book
        # fields='__all__'  # 所所有字段
        exclude = ['id', 'pu_date']  # 排除字段 和 fields 不能一起使用
        
        
class BookView(GenericAPIView): # 继承APIView
    # 查询集
    queryset = Book.objects.all()
    # 序列化器
    serializer_class = BookSerializers

    def get(self, request, *args, **kwargs):
        # get_queryset 获取全局queryset变量
        # get_serializer_class() # 获取全局序列化器
        # serializer = self.get_serializer_class()(instance=self.get_queryset(), many=True)  # 多个模型类对象

        # get_serializer() 获取序列化器对象
        serializer = self.get_serializer(instance=self.get_queryset(), many=True)

        # 返回json数据
        return Response(serializer.data)
	 def post(self, request):
        print("Request POST", request.data)
        serializer = self.get_serializer(data=request.data)
        # 校验数据  成功 serializers.validated_data   失败 serializer.errors
        if serializer.is_valid():  # 全部校验成功返回True
            # 保存到数据库
            # Book.objects.create(**serializer.data)
            serializer.save()
            return Response(serializer.data)
        else:
            return Response(serializer.errors)
       
    
class BookDetailView(GenericAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    def get(self, request, pk):
        # get_object() 获取模型类对象
        serializer = self.get_serializer(instance=self.get_object())
        return Response(serializer.data)

    def put(self, request, pk):
        serializer = self.get_serializer(data=request.data, instance=self.get_object())
        if serializer.is_valid():  # 校验数据
            # 保存到数据库
            serializer.save()
            return Response(serializer.data)

    def delete(self, request, pk):
        self.get_object().delete()
        return Response()
```

![image-20240102205829232](assets/image-20240102205829232.png)
![image-20240103143335233](assets/image-20240103143335233.png)





### 3.3 mixins 视图扩展类

```python
from rest_framework import serializers
from rest_framework.generics import GenericAPIView
from rest_framework.mixins import ListModelMixin, CreateModelMixin, RetrieveModelMixin, UpdateModelMixin, \
    DestroyModelMixin
from sers.models import Book

class BookSerializers(serializers.ModelSerializer):
    # 自定义字段
    date = serializers.DateField(source='pu_date')

    class Meta:
        # 指定关联的模型类
        model = Book
        # fields='__all__'  # 所所有字段
        exclude = ['id', 'pu_date']  # 排除字段 和 fields 不能一起使用


class BookView(ListModelMixin, CreateModelMixin, GenericAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    def get(self, request):
        return self.list(request)

    def post(self, request):
        return self.create(request)


class BookDetailView(RetrieveModelMixin, UpdateModelMixin, DestroyModelMixin, GenericAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    def get(self, request, pk):
        return self.retrieve(request, pk)

    def put(self, request, pk):
        return self.update(request, pk)

    def delete(self, request, pk):
        return self.destroy(request, pk)
```

![image-20240103151231464](assets/image-20240103151231464.png)


![image-20240103151247987](assets/image-20240103151247987.png)






### 3.4 mixin 混合类

**进一步封装**

```python
from rest_framework import serializers
from rest_framework.generics import ListCreateAPIView, RetrieveUpdateDestroyAPIView
from sers.models import Book

class BookSerializers(serializers.ModelSerializer):
    date = serializers.DateField(source='pu_date')
    class Meta:
        model = Book
        exclude = ['id', 'pu_date']  
        
        
class BookView(ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

class BookDetailView(RetrieveUpdateDestroyAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
```

![image-20240103153716500](assets/image-20240103153716500.png)




### 3.5 ViewSet  重写路由规则

**ViewSetMixin的子类，可以重写路由规则**

```python
urls.py
	urlpatterns = [
    path('books/', views.BookALLView.as_view({"get": "get_all", 'post': 'post_obj'})),
    re_path(r'books/(?P<pk>\d+)', views.BookALLView.as_view({'get': 'get_obj', 'put': 'update_obj', 'delete': 'delete_obj'}))
]



views.py
from rest_framework.viewsets import ViewSet


class BookALLView(ViewSet): #重写路由规则
    def get_all(self, request):
        return Response('获取所有资源')

    def post_obj(self, request):
        return Response('添加资源')

    def get_obj(self, request, pk):
        return Response('获取单个资源')

    def update_obj(self, request, pk):
        return Response('编辑单个资源')

    def delete_obj(self, request, pk):
        return Response('删除单个资源')

```

![image-20240103173256696](assets/image-20240103173256696.png)



### 3.6 GenericViewSet

```python
urls.py
    urlpatterns = [
        path('books/', views.BookALLView.as_view({"get": "list", 'post': 'create'})),
        re_path(r'books/(?P<pk>\d+)', views.BookALLView.as_view({'get': 'retrieve', 'put': 'update', 'delete': 'destroy'}))
    ]

    
views.py
	from rest_framework.mixins import ListModelMixin, CreateModelMixin, RetrieveModelMixin, UpdateModelMixin, \
    DestroyModelMixin
	from rest_framework.viewsets import GenericViewSet

    class BookALLView(ListModelMixin, CreateModelMixin, RetrieveModelMixin, UpdateModelMixin, \
                      DestroyModelMixin, GenericViewSet):
        queryset = Book.objects.all()
        serializer_class = BookSerializers
```

![image-20240103175522282](assets/image-20240103175522282.png)





### 3.6 ModelViewSet  **超NB**

```python
urls.py
    urlpatterns = [
        path('books/', views.BookALLView.as_view({"get": "list", 'post': 'create'})),
        re_path(r'books/(?P<pk>\d+)', views.BookALLView.as_view({'get': 'retrieve', 'put': 'update', 'delete': 'destroy'}))
    ]


    
views.py
	from rest_framework.viewsets import ModelViewSet
    from rest_framework import serializers
    from rest_framework.viewsets import ModelViewSet
    from sers.models import Book
    
    class BookSerializers(serializers.ModelSerializer):
        date = serializers.DateField(source='pu_date')
        class Meta:
            model = Book
            exclude = ['id', 'pu_date']
            
            
    class BookALLView(ModelViewSet):
        queryset = Book.objects.all()
        serializer_class = BookSerializers
```

![image-20240103180614974](assets/image-20240103180614974.png)





## 4. 路由组件



### 4.1 自动生成路由

1. DefaultRouter()   生成根路由，显示可访问的路由
2. routers.SimpleRouter() 

```python
from rest_framework import routers
from sers import views

app_name = 'sers'

# 生成路由
router = routers.DefaultRouter()
router.register('books', views.BookALLView)  # 前缀、视图、别名
urlpatterns = [
    
]

# 添加路由
urlpatterns += router.urls
```

根目录

![image-20240104161152071](assets/image-20240104161152071.png)


![image-20240103184219970](assets/image-20240103184219970.png)



### 4.2 视图类中派生的方法，自动生成路 action

​	methods 列表 请求方法
​	detail  是否携带pk
​	url_path  生成的路由

```python
from rest_framework.response import Response
from rest_framework.viewsets import ModelViewSet


class BookALLView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
	
    # 自己扩展的方法(派生)
    @action(methods=["GET"], detail=False)
    def login(self, request):
        response = super().list(request)
        data = {"data": response.data, "msg": "访问成功", "code": 100}
        return Response(data) 
```

![image-20240104180050186](assets/image-20240104180050186.png)



```python
class BookALLView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    @action(methods=["GET"], detail=True, url_path='search')
    def login(self, request, pk):
        response = super().retrieve(request, pk)
        data = {"data": response.data, "msg": "访问成功", "code": 100}
        return Response(data)
```

![image-20240104180442731](assets/image-20240104180442731.png)





## 5. 认证组件



### 5.1 登录功能

models.py
```
from django.db import models

# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=32)

    class Meta:
        db_table = 'User'

class Token(models.Model):
    user = models.OneToOneField(to='user', on_delete=models.SET_NULL, null=True)
    token = models.CharField(max_length=64)

    class Meta:
        db_table = 'Token'
```

views.py
```python
class UserView(APIView):
    def get(self, request):
        return Response('ok')

    def post(self, request):
        username = request.data.get('username')
        password = request.data.get('password')
        user = models.User.objects.filter(username=username, password=password).first()
        res = {'code': 101, 'msg': '用户名或密码不粗糙你在'}  # 设置成这样可以一定程度防爬虫

        if user:  # 如果有当前登录对象
            token = uuid.uuid4()  # 生成一个随机字符串
            models.Token.objects.update_or_create(defaults={'token': token}, user=user)  # 存在则更新，不存在则创建
            res['msg'] = '登陆成功！'
            res['token'] = token
            return Response(res)
        else:
            return Response(res)
```

**查找顺序：视图类中—>配置文件—>内置配置文件**

![image-20240104210126206](assets/image-20240104210126206.png)


### 5.2 认证类

authenticate.py

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed

from user import models


class LoginRequire(BaseAuthentication):
    # 重写认证方法
    def authenticate(self, request):
        # 取出用户携带的token
        token = request.query_params.get('token')
        token_obj = models.Token.objects.filter(token=token).first()
        if token_obj:  # 用户登录成功才会携带token
            user = token_obj.user  # 当前登录对象
            return user, token   # 第一个值是request.user
        else:
            raise AuthenticationFailed('请登录！')

```

在views使用认证类
```python
from user.authenticate import LoginRequire

class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    # 局部使用
    authentication_classes = [LoginRequire,]
```

未登录
![image-20240105153816752](assets/image-20240105153816752.png)

携带token

![image-20240105154150818](assets/image-20240105154150818.png)






## 6. 权限组件



### 6.1 简单使用

models.py
```python
class User(models.Model):
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=32)
    is_super = models.IntegerField(default=0)  # 0表示普通用户 1表示超级用户

    class Meta:
        db_table = 'User'
```

permission.py

```python
from rest_framework.permissions import BasePermission

class Permission(BasePermission):
    # 认证组件走完进入权限组件
    # 重写has_permission
    def has_permission(self, request, view):
        is_super = request.user.is_super
        if is_super:
            return True
        else:
            return False
```

views.py
```python
from user.authenticate import LoginRequire
from user.authenticate import Permission


class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    authentication_classes = [LoginRequire, ]
    permission_classes = [Permission, ]
```

![image-20240105170243909](assets/image-20240105170243909.png)



设置当前权限为0
![image-20240105170142577](assets/image-20240105170142577.png)

### 6.2 Session的执行流程

![image-20240105181334304](assets/image-20240105181334304.png)

**Django设置session的流程**

1. 随机生成一个随机字符串作为session_key
2. 将sessionid作为键，session-key作为值构成的键值对保存在浏览器的cookie
3. 将session-key和session数据保存在数据库中
   

**简单使用**
views.py

```python
from rest_framework.response import Response
from rest_framework.views import APIView
from user import models


# Create your views here.

class UserView(APIView):
    def get(self, request):
        return Response('ok')

    def post(self, request):
        username = request.data.get('username')
        password = request.data.get('password')
        user = models.User.objects.filter(username=username, password=password).first()
        res = {'code': 101, 'msg': '用户名或密码不粗糙你在'}

        # 第一次登录，设置session
        if user:  # 如果有当前登录对象
            # 设置session
            request.session['user'] = user.username
            # 提前将session保存到数据库中
            request.session.save()
            # 返回生成的随机字符串
            res['token'] = request.session.session_key
            res['msg'] = '登陆成功！'
            return Response(res)
        else:
            return Response(res)
```

authenticate.py
```python
class LoginRequire(BaseAuthentication):
    # 重写认证方法
    def authenticate(self, request):
        # 取出用户携带的token
        token = request.query_params.get('token')
        # 取出django_session表中的session_key
        django_token = request.session.session_key

        if django_token == token:  
            return #可以返回空
        else:
            raise AuthenticationFailed('请登录！')
```

![image-20240105185629608](assets/image-20240105185629608.png)
![image-20240105190552772](assets/image-20240105190552772.png)



## 7. 频率组件

**限制访问频率  1.用户id  2.ip地址**

throttle.py
```python
from rest_framework.throttling import SimpleRateThrottle


class Throttle(SimpleRateThrottle):
    scope = "ip_throttle"

    def get_cache_key(self, request, view):
        # 返回谁就以谁为限制
        print(request.META)
        return request.META.get('REMOTE_ADDR')
```

views.py
```python
class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    authentication_classes = [LoginRequire, ]
    throttle_classes=[Throttle,]
```

settings.pu
```pyhton
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'ip_throttle': '5/minute',  # 每分钟最多5次请求,与设置的scope一致
    },
}

```

![image-20240106113641154](assets/image-20240106113641154.png)



## 8. 过滤和排序

**查询所有才需要过滤（根据条件过滤），排序（按某规则排序）**

### 8.1 简单使用

views.py
```python
from rest_framework.filters import SearchFilter, OrderingFilter


class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    filter_backends = [SearchFilter, OrderingFilter]
    # 设置以什么字段查询
    search_fields = ['title']
    # 设置以什么字段排序
    ordering_fields = ['price']
```

![image-20240106121533772](assets/image-20240106121533772.png)
![image-20240106121607467](assets/image-20240106121607467.png)



## 9. 分页

### 9.1 基本分页 

pagination.py
```python
from rest_framework.pagination import PageNumberPagination


class PageNumber(PageNumberPagination):
    page_size = 2  # 每页显示条数
    page_query_param = 'page'  # 查询时有的参数
    page_size_query_param = 'size'  # 更改返回条数
    max_page_size = 5  # 每页最多显示条数
```

views.py

```python
from .pagination import PageNumber


class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    pagination_class = PageNumber
```

![image-20240106170106358](assets/image-20240106170106358.png)


![image-20240106170339129](assets/image-20240106170339129.png)


### 9.2 偏移分页

pagination.py
```python
from rest_framework.pagination import LimitOffsetPagination

class LimitOffset(LimitOffsetPagination):
    default_limit = 2  # 每页显示条数
    limit_query_param = 'limit'  # limit的参数
    offset_query_param = 'offset'  # 偏移查询参数
    max_limit = 5  # 每页最大显示条数
```

views.py

```python
from .pagination import LimitOffset


class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    pagination_class = LimitOffset
```

![image-20240106171827730](assets/image-20240106171827730.png)


![image-20240106172111387](assets/image-20240106172111387.png)


### 9.3 游标分页

pagination.py
```python
from rest_framework.pagination import CursorPagination

class Cursor(CursorPagination):
    cursor_query_param = 'cursor'  # 查询条件
    page_size = 2  # 每页显示条数
    ordering = 'price'  # 排序
    page_size_query_param = 'size'  # 每页显示多少条的参数
    max_page_size = 5  # 每页最大显示多少条

```

views.py

```python
from .pagination import Cursor

class BookView(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    pagination_class = Cursor
    
    
优点：速度快，数据越大，优势越明显
缺点：只能前一页和后一页，不能直接跳到某一页
```

![image-20240106174504133](assets/image-20240106174504133.png)


### 9.4 基于APIView生成分页

```python
from rest_framework.response import Response
from .pagination import PageNumber

class BookGetView(APIView):
    def get(self, request):
        book_list = Book.objects.all()
        # 创建分页对象
        pagination = PageNumber()
        book_list2 = pagination.paginate_queryset(book_list, request, self)
        # 序列化 分页的数据
        ser = BookSerializers(instance=book_list2, many=True)

        data = {"count": pagination.page.paginator.count, 'pre': pagination.get_previous_link(),
                'next': pagination.get_next_link(), 'data': ser.data}
        return Response(data)
        # return pagination.get_paginated_response(ser.data)
    

    
    
def get_paginated_response(self, data):
	return Response(OrderedDict([
            ('count', self.page.paginator.count),
            ('next', self.get_next_link()),
            ('previous', self.get_previous_link()),
            ('results', data)
        ]))
```

![image-20240106183656421](assets/image-20240106183656421.png)




## 10. 全局异常

utils.py
```python
from rest_framework.response import Response
from rest_framework.views import exception_handler


def my_exception_handler(exc, context):
    # 先调用REST framework默认的异常处理方法获得标准错误响应对象
    res = exception_handler(exc, context)

    # 在此处补充自定义的异常处理
    if res:
        data = {'code': 500, 'msg': '错误', 'detail': res.data}
        return Response(data)
    else:
        data = {'code': 500, 'msg': '未知错误'}
        return Response(data)

```

修改配置

```PYTHON
 REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'utils.my_exception_handler',
}
```

![image-20240106135342212](assets/image-20240106135342212.png)
s



## 11. 自动生成文档接口

**后端人员写好接口，写好接口文档，提供给前端人员开发使用**

**公司主流**

1. 使用word、md写，提交到git
2. 有接口平台，后端人员在接口平台录入
3. 后端项目自动生成接口文档(不太友好) coreapi、swagger

```python
1.安装依赖
	pip install coreapi
    
2.设置文档访问路径
	from rest_framework.documentation import include_docs_urls

	urlpatterns = [
        ...
        path('docs/', include_docs_urls(title='站点页面标题'))
     ]
    
3.配置接口文档
	REST_FRAMEWORK = {
    # 接口文档
    'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.AutoSchema',
	}
```

views.py

```
from .pagination import PageNumber


class BookSerializers(serializers.ModelSerializer):
    # 自定义字段
    date = serializers.DateField(source='pu_date',help_text='日期')

    class Meta:
        # 指定关联的模型类
        model = Book
        # fields='__all__'  # 所所有字段
        exclude = ['id', 'pu_date']  # 排除字段 和 fields 不能一起使用
        extra_kwargs = {
            'title': {'help_text': '书名'},
            'price': {'help_text': '价格'}
        }


class BookView(ModelViewSet):
    '''
    list:
    获取所有书籍信息
    create:
    新增一本书籍
    '''
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    pagination_class = PageNumber
```

![image-20240107112333564](assets/image-20240107112333564.png)






## 12. RBAC

### 12.1 简介

**基于角色的访问控制**

应用：

1. crm：客户关系管理
2. oa系统
3. 自动化运维项目

原理：权限和角色相关联，用户通过成为适当的角色而获得角色对应的权限，简化了权限的管理，权限赋予给角色，而角色又赋予个用户

```python
django中的rbac   auth 6张表实现
	- 用户表
    - 权限表
    - 角色表
    
    - 用户角色关系表  多对多
    - 权限角色关系表  多对多
    - 用户权限关系表  多对多
```



### 12.2 django-simpleui

```python
1. 安装django-simpleui

2. 配置
	INSTALLED_APPS = [
    'simpleui',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    ]
```

![image-20240107133204625](assets/image-20240107133204625.png)





## 13. JWT

  **json web token  一种前后端的认证方式，不需要访问数据库，服务器页不需要存储token，签发和校验都是通过代码实现的**
![image-20240107160517191](assets/image-20240107160517191.png)





### 13.1 构成和工作原理

**构成：JWT就是一段字符串，由三段信息构成的，将这三段信息文本用.链接一起就构成了Jwt字符串。**

```python
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

```python
header 头
	- 声明类型
    - 声明加密算法
        {
         'typ': 'JWT',
         'alg': 'HS256'
        }
然后将头部进行base64加密，构成了第一部分。

payload 荷载 
	- 存储有效信息的地方
		{
          "sub": "1234567890",
          "name": "John Doe",
          "expire": 3600
    	}
然后将其进行base64加密，得到JWT的第二部分。


signature 签证信息
	- header (base64后的)
	- payload (base64后的)
	- secret 秘钥
这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密，然后就构成了jwt的第三部分
```



**工作原理：用户注重和登录成功后签发token，根据客户端带token的请求，取出第一部分和第二部分结合秘钥加密后与第三部分进行比较**

1. 根据登录请求提交来的 账号 + 密码 + 设备信息 签发 token

2. 根据客户端带token的请求 反解出 user 对象



### 13.2 drf中的使用

```python
安装 pip install djangorestframework-simplejwt
```

**签发**

```python
使用的用户表是  django auth 的User表

url.py
	from rest_framework_simplejwt.views import token_obtain_pair, token_verify, token_refresh

    urlpatterns = [
        path('login/', token_obtain_pair),  # 处理用户的登录请求
        path('verify/', token_verify),  # 验证提供的JWT令牌是否有效
        path('refresh/', token_refresh),  # 刷新现有的JWT令牌
    ]
    
    
token的修改和过时，都会导致token失效
```

![image-20240107185339898](assets/image-20240107185339898.png)


![image-20240107185438474](assets/image-20240107185438474.png)


**校验**

```python
from .pagination import PageNumber
from rest_framework_simplejwt.authentication import JWTAuthentication
from rest_framework.permissions import IsAuthenticated


class BookView(ModelViewSet):
    '''
    list:
    获取所有书籍信息
    create:
    新增一本书籍
    '''
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    pagination_class = PageNumber
    
    authentication_classes = [JWTAuthentication, ]
    permission_classes = [IsAuthenticated, ]
    
    
由于设置了请求头格式，我们需要在请求中添加Authorization:"Bearer [token]"
```

![image-20240107190634128](assets/image-20240107190634128.png)
