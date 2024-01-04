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

### 3.1 GenericAPIView

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





### 3.2 mixins 视图扩展类

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





### 3.3 mixin 混合类

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



### 3.3 ViewSet  重写路由规则

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


### 3.4 GenericViewSet

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




### 3.5 ModelViewSet  **超NB**

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

![image-20240104210126206](assets/image-20240104210126206.png)


