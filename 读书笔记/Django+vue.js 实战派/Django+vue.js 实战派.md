[TOC]

## 1. 走进Django

### 1.1  MVC模式和MTV模式

MVC模式

- 模型(Model):负责各个功能的实现，包含模型实体类和业务处理类
- 视图(View): 负责页面的显示和用户的交互
- 控制器(Controller): 用于将用户请求转发给相应的模型进行处理并返回

图示
![image-20231130191718423](assets/image-20231130191718423.png)



MTV模式

- 模型(Model): 负责业务对象和数据库的关系映射(orm)
- 模板(Template): 负责页面的显示和用户的交互
- 视图(View): 负责业务逻辑的处理，并在适当的时候调用Model和Template

图示
![image-20231130192050190](assets/image-20231130192050190.png)


俩者的对比

![image-20231130192427554](assets/image-20231130192427554.png)





### 1.2 开发第一个Django项目

创建项目
```sql
django-admin startproject myshop
```

![image-20231130193125291](assets/image-20231130193125291.png)
创建app01的应用

```shell
切换到和manage.py文件同级目录
python manage.py startapp app01
```

![image-20231130193403895](assets/image-20231130193403895.png)
创建应用后再配置文件注册app应用

```python
settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # 注册创建的app应用
    'app01',
]
```

处理视图 app01/views.py
```sql
from django.shortcuts import render

# Create your views here.
# 定义视图函数
def index(request):
    return render(request, '1/index.html')
```

处理url myshop/url.py
```python
from django.contrib import admin
from django.urls import path
from app01 import views

urlpatterns = [
    path('admin/', admin.site.urls),

    path('index/', views.index),#访问路由，指定视图函数
]
```

创建目录和模板文件，在manage.py文件的同级目录创建一个templates目录，并在该目录下创建一个'1'目录，在创建一个index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div style="color:red;font-size:24px;">你好！ django</div>
</body>
</html>
```

![image-20231130201143290](assets/image-20231130201143290.png)


在全局文件中对模板目录进行注册
```python
import os
TEMPLATES = [
    {
        'DIRS': [os.path.join(BASE_DIR,'templates')],
    }
]
```

运行程序
```python
python manage.py runserver
```

![image-20231130201204026](assets/image-20231130201204026.png)





## 2. 网站的入口-Django的路由和视图

### 2.1 路由

- 路由系统的基本配置
  
  myshop/url.py
  
  ```python
  from django.contrib import admin
  from django.urls import path
  from app01 import views
  
  #该urlpatterns列表定义的路由为整个项目的根路由
  urlpatterns = [
      path('admin/', admin.site.urls),#默认路由
  
      path('index/', views.index),#访问路由，指定视图函数
  ]
  
  #path(路由,视图,别名)
  ```
  
  
  
- 路由包含，简化项目复杂度
  
  随着业务增多，路由规则越来越复杂，可以为每个应用创建一个urls.py文件，把相关的路由配置放在每个应用的url.py文件中
  
  1. 路由匹配规则
     项目的urls.py文件中，urlpatterns列表会从上到下进行匹配
  
     - 匹配成功，调用path()函数的第2个参数指定的视图，且不会继续向下匹配
     - 匹配失败，返回404
     - 如果应用中定义了子路由，根路由使用include('应用名.urls')来加载子路由，如果url第一部分被匹配，其余部分在子路由中进行匹配。
  
  2. 示例
     myshop/urls.py
  
     ```PYTHON
     from django.contrib import admin
     from django.urls import path, include
     from app01 import views
     
     urlpatterns = [
         path('admin/', admin.site.urls),
     
         path('index/', views.index),#访问路由，指定视图函数
         path('',include('app01.urls')),
         path('',include('app02.urls'))
     ]
     ```
  
     app02/urls.py
     ```python
     from django.urls import path
     from app02 import views
     
     urlpatterns = [
         path('app02/index/', views.index)
     ]
     ```
  
     app02/views.py
     ```python
     from django.http import HttpResponse
     from django.shortcuts import render
     
     
     # Create your views here.
     def index(request):
         return HttpResponse('app02中的index方法')
     ```
  
     运行结果
     ![image-20231130203731084](assets/image-20231130203731084.png)
     
     
  
- re_path()方法正则匹配复杂路由
  语法格式
  
  ```python
  (?P<name>pattern)
  
  name是匹配的字符串名称
  pattern是要匹配的模式
  ```
  
  示例
  app02/urls.py

  ```python
  from django.urls import path, re_path
  from app02 import views
  
  urlpatterns = [
      re_path(r'app02/list/(?P<year>\d{4})', views.article_list),
      re_path(r'app02/page/(?P<page>\d+)&key=(?P<key>\w+)', views.article_list),
  ]
  ```
  
  app02/views.py
  
  ```python
  def article_list(request, year):
      return HttpResponse('app02中的article_list方法,参数year，值为' + str(year))
  
  
  def article_page(request, page, key):
      return HttpResponse('app02中的article_page方法,参数page,key，值为' + str(page) + ':' + str(key))
  ```

  运行结果
  ![image-20231201104817090](assets/image-20231201104817090.png)
  
  ![image-20231201105444680](assets/image-20231201105444680.png)
  
- 解析路由参数

  1. 编写url带参数的路由
     app02/urls.py

     ```python
     urlpatterns = [
         path('app02/show/<int:id>', views.show)
     ]
     ```

     app02/views.py

     ```
     def show(request, id):
         return HttpResponse('app02中的show方法,参数id，值为' + str(id))
     ```

     运行结果
     ![image-20231130204810124](assets/image-20231130204810124.png)

  2. url参数
     语法格式

     ```python
     <参数数据类型 : 参数名称>
     ```

     数据类型
     ![IMG_20231130_205042](assets/IMG_20231130_205042.jpg)

  3. 示例
     app02/views.py

     ```python
     urlpatterns = [
         path('app02/article/<uuid:u>', views.show_uuid),
         path('app02/article/<slug:s>', views.show_slug),
     ]
     ```

     app02/views.py

     ```python
     def show_uuid(request, u):
         return HttpResponse('app02中的show方法,参数u，值为' + str(u))
     
     
     def show_slug(request, s):
         return HttpResponse('app02中的show方法,参数s，值为' + str(s))
     ```

     运行过程
     ![image-20231130210258039](assets/image-20231130210258039.png)

     ![image-20231130210334405](assets/image-20231130210334405.png)
     

- 反向解析路由
  在路由配置中，给路由命名，然后在视图和模板中反向解析出url，只要name不变，url可以任意改变
  app02/urls.py

  ```python
  urlpatterns = [
      path('app02/url_reverse/',views.url_reverse,name='app02_url_reverse'),
  ]
  ```

  app02/views.py
  ```python
  from django.http import HttpResponse
  from django.shortcuts import render
  from django.urls import reverse
  
  def url_reverse(request):
      # 使用reverse()方法反向解析
      print(reverse('app02_url_reverse'))
      return render(request,'2/url_reverse.html')
  ```

  运行结果

  ![image-20231201123751690](assets/image-20231201123751690.png)
  ![image-20231201123822926](assets/image-20231201123822926.png)



### 2.2 视图

1. 请求对象
   主要的方法和属性
   ![image-20231201131929091](assets/image-20231201131929091.png)

   app02/urls.py
   ```python
   from django.urls import path, re_path
   from app02 import views
   
   urlpatterns = [
       re_path(r'app02/test_get', views.test_get)
   ]
   ```

   app02/views.py
   ```python
   def test_get(request):
       print(request.get_host(),'域名+端口')
       print(request.get_raw_uri(),'全部路径，包含参数')
       print(request.path,'访问路径，不包含参数')
       print(request.get_full_path(),'访问路径，包含参数')
       print(request.method,'获取请求方式')
       print(request.GET,'获取get请求参数')
       print(request.META['REMOTE_ADDR'],'客户端ip地址')
       print(request.GET.get('key'), '获取key参数')
       return HttpResponse('ok')
   ```

   浏览器输入http://127.0.0.1:8000/app02/test_get?key=2

   ![image-20231201131739186](assets/image-20231201131739186.png)
   

   app02/urls.py
   ```python
   urlpatterns = [
       re_path(r'app02/test_post', views.test_post)
   ]
   ```

   app02/views.py
   ```python
   def test_post(request):
       print(request.method,'获取请求方法')
       print(request.POST.get('username'))
       return render(request, '2/test_post.html')
   ```

   ![image-20231201133003642](assets/image-20231201133003642.png)
   
2. 响应对象
   每个视图都会返回一个响应对象，该对象包含客户端的所有数据

   | 属性         | 含义                                |
   | ------------ | ----------------------------------- |
   | content      | 返回的内容                          |
   | status_code  | 返回的http响应状态码                |
   | content-type | 返回数据的mime类型，默认为text/html |

   app02/urls.py
   ```python
   urlpatterns = [
       path('app02/test_response', views.test_response)
   ]
   ```

   app02/views.py
   ```python
   def test_response(request):
       response = HttpResponse('hello Django!')
       response.write('<br>')
       response.write(response.content)  # 写入返回的内容
       response.write('<br>')
       response.write(response['Content-Type'])
       response.write('<br>')
       response.write(response.charset)
       response.write('<br>')
       return response
   ```

   运行结果
   ![image-20231201135042723](assets/image-20231201135042723.png)

3. 视图处理函数的使用

   - render( )函数实现的页面渲染

     ```python
     根据模版文件和传递的上下文字典对象，生成一个HttpResponse对象并返回
     
     from django.shortcuts import render
     render( request,template_name,context)
     
     request :请求对象
     template_name: 渲染的模版文件
     context ： 上下文字典对象，传递个模板的数据
     ```

     

   - redirect() 函数实现页面重定向

     1. 路由反解析重定向
        app02/views.py

        ```python
        def test_redirect(request):
            return redirect('app02_url_reverse')
        ```

        app02/urls.py

        ```python
        urlpatterns = [
            path('app02/test_redirect', views.test_redirect),
        ]
        ```

        ![image-20231201143555031](assets/image-20231201143555031.png)

     2. 传入一个绝对的url
        ```python
        def test_redirect(request):
            return redirect('https://www.baidu.com')
        ```

        ![image-20231201143850494](assets/image-20231201143850494.png)

4. 视图类
   视图类将每个方法的处理逻辑变成视图类中的单个方法，使得程序的逻辑变得更加简单清晰

   1. 视图类的使用-View

      app02/views.py

      ```python
      from django.views import View
      
      
      class IndexPageView(View):
          def get(self, request):
              return HttpResponse('get请求')
      
          def post(self, request):
              return HttpResponse('post请求')
      ```

      app02/urls.py

      ```python
      from app02.views import IndexPageView
      urlpatterns = [
          path('app02/index', views.IndexPageView.as_view())
      ]
      ```

   2. 通用视图类 --TemplateView 
      app02/views_class.py

      ```python
      from django.views.generic import TemplateView
      
      
      class TestTemplateView(TemplateView):
          # 设置模版文件
          template_name = '2/test_template.html'
      
          # 重写父类方法
          def get_context_data(self, **kwargs):
              context = super().get_context_data()
              context['info']='该变量可以传递到模板'
              return context
      ```

      app02/urls.py

      ```python
      from app02.views_class import TestTemplateView
      
      urlpatterns = [
          path('app02/test_template', TestTemplateView.as_view())
      ]
      ```

      运行结果
      ![image-20231201150317869](assets/image-20231201150317869.png)

   3. 列表视图类-ListView
      将数据表的数据以列表的形式展现
   
   4. 详细视图类-DetailView
      将数据表的数据以详细视图展示





## 3. 页面展现-基于Django模版

Django模版技术可以分为俩部分

- 静态部分，html、css、javascript
- 动态部分，DJango模版语言包括模板变量、模板标签、模板过滤器

### 3.1 模板语言

1. 模板变量


   可以看作html文件中的占位符，当Django模板执行时，会使用模板变量实际值对其进行替换

   模板变量表示
   ```python
   {{变量名}}
   模板变量可以是字典、列表、类对象
   ```

   示例
   ```python
   app03/urls.py
   from django.urls import path, include
   from app03 import views
   
   urlpatterns = [
       path('app03/var', views.var),
   ]
   
   
   app03/views.py
   from django.shortcuts import render
   
   # Create your views here.
   def var(request):
       lists = ['Java', 'Python', 'C', 'C#', 'Javascript']  # 列表对象
       dicts = {'name': 'liudong', 'age': '18', 'sex': 'male'}  # 字典对象
       context = {'lists': lists, 'dicts': dicts}
       return render(request, '3/var.html', context=context)
   ```

   3/var.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   {{ lists }}
   <table border="1">
       <tr>
           <td>{{ lists.0 }}</td>
           <td>{{ lists.1 }}</td>
           <td>{{ lists.2 }}</td>
           <td>{{ lists.3 }}</td>
           <td>{{ lists.4 }}</td>
       </tr>
   </table>
   {{ dicts }}
   <table border="1">
       <tr>
           <td>{{dicts.name }}</td>
           <td>{{dicts.age }}</td>
           <td>{{dicts.sex }}</td>
       </tr>
   </table>
   </body>
   </html>
   ```

   ![image-20231201202954718](assets/image-20231201202954718.png)
   

2. 模版标签
   用{% 标签名 %}

   常见模版标签
   ![image-20231201203333530](assets/image-20231201203333530.png)

   循环模板标签
   ![image-20231201203630583](assets/image-20231201203630583.png)

   示例
   app03/views.py

   ```python
   def for_label(request):
       lists = ['Java', 'Python', 'C', 'C#', 'Javascript']
       return render(request, '3/for_label.html', {'lists': lists})
   ```

   app03/urls.py
   ```python
   urlpatterns = [
       path('app03/for_label', views.for_label),
   ]
   ```

   3/for_label.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   {{ lists }}
   <hr>
   {% for li in lists %}
       {{ li }}
   {% empty %}
       {{ 当可迭代对象为空 }}
   {% endfor %}
   
   <hr>
   {% for li in lists %}
       {{ li }} 当前索引{{ forloop.counter }}
       <br>
   {% empty %}
       {{ 当可迭代对象为空 }}
   {% endfor %}
   
   </body>
   </html>
   ```

   运行结果
   ![image-20231201204737503](assets/image-20231201204737503.png)

3. 模板过滤器
   语法格式

   ```shell
   {{变量名|过滤器:参数}}
   ```

   常用过滤器
   ![image-20231201205207814](assets/image-20231201205207814.png)

   示例
   app03/views.py

   ```python
   from datetime import datetime
   def filter(request):
       str1 = 'abcdefg'
       str2 = 'ABCDEFG'
       slice_str = '1234567890'
       time_str = datetime.now()
       return render(request, '3/filter.html', {'str1': str1, 'str2': str2, 'time_str': time_str})
   ```

   app03/urls.py

   ```python
   urlpatterns = [
       path('app03/filter', views.filter),
   ]
   ```

   filter.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   小写转大写 {{ str1|upper }}<hr>
   大写转小写 {{ str2|lower }}<hr>
   切片操作 {{ slice_str|slice:'2:4' }}<hr>
   时间格式化 {{ time_str|date:'Y-m-d G:i:s' }}
   </body>
   </html>
   ```

   运行结果
   ![image-20231201210524588](assets/image-20231201210524588.png)

### 3.2 模板高级用法

1. 模板转义
   Django模板会对 *html* 标签和 *javascript* 标签进行转义，为了代码安全，避免用户提交了攻击性代码
   app03/views.py

   ```python
   def html_filter(request):
       html_ddr = '<h1>大大大<h2>小小</h2></h1>'
       html_javascript = '<script>document.write("js脚本写入")</script>'
       return render(request, '3/html_filter.html', {'html_ddr': html_ddr, 'html_javascript': html_javascript})
   
   ```

   app03/url.py
   ```python
   urlpatterns = [
       path('app03/html_filter', views.html_filter),
   ]
   ```

   htnl_filter.html
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   关闭模版转义 {{ html_ddr }}
   开启模版转义 {{ html_ddr|safe }}
   <hr>
   关闭模版转义 {{ html_javascript }}
   <br>
   开启模版转义 {{ html_javascript|safe }}
   </body>
   </html>
   ```

   运行结果

   ![image-20231201211756063](assets/image-20231201211756063.png)
   
