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



## 2. CBV源码解析

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
  