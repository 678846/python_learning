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
      
  
      
  BookView.as_view() --> view --> dispatch() --> handler()
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

### 2.3 配合前端修改相关参数

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

```python
from rest_framework.generics import GenericAPIView

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
```

![image-20240102205829232](assets/image-20240102205829232.png)
