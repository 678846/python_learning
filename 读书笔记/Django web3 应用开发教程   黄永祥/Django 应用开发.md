## 补充

### 1. 文件下载 

```python
from django.http import JsonResponse, FileResponse, Http404

def download(request):
    try:
        f = open(r'C:\Users\user-liu\Desktop\girl.jpg', 'rb')
        ret = FileResponse(f, as_attachment=True, filename='girl.jpg')
        return ret
    except Exception:
        return Http404('Download error')
    

as_attachment=True,   开启文件下载功能，并设置文件后缀名
filename='girl.jpg'	  设置下载的文件名
```



### 2.  配置jinja2模板引擎

- 安装jiaja2模块  pip install jinja2

- settings.py 同级目录下创建一个jinja2.py 文件
  ```python
  from django.contrib.staticfiles.storage import staticfiles_storage
  from django.urls import reverse
  from jinja2 import Environment
  
  
  def jinja2_environment(**options):
      env = Environment(**options)
      env.globals.update({
          'static': staticfiles_storage.url,
          'url': reverse,
      })
      return env
  ```

- settings.py 配置模板引擎
  ```python
  TEMPLATES=[
      {
          'BACKEND': 'django.template.backends.jinja2.Jinja2',
          'DIRS': [os.path.join(BASE_DIR, 'templates')],
          'APP_DIRS': True,
          'OPTIONS': {
              'environment': 'crm.jinja2.jinja2_environment',
          },
      },
  ]
  ```

  

### 3. 资源配置

- 静态资源配置
  ```tex
  static_url :设置静态资源的路由地址
  staticfiles_dirs  :将项目的静态资源文件夹绑定到Django中
  ```

- 媒体资源 

  ```text
  media_url :设置媒体资源的路由地址
  media_root :将项目的媒体资源文件夹绑定到Django中
  ```

  

## 11. 常用的web应用程序

### 11.1 会话控制

**Django内置的会话控制简称为session。当用户第一次访问时，网站的服务器自动创建一个session对象存储着用户的数据信息，作为该用户在网站的一个身份凭证。**

#### 11.1.1 会话配置和操作

- Session 和 Cookie 的比较

  1. Session存储在服务器，Cookie存储在客户端，理论上Session比较安全
  2. 当获取某用户的Session数据，先从Cookie中获取sessionid，然后根据sessionid在服务器中找到相应的session

- Session保存方式
  ```python
  # session 默认的保存方式，无需设置
  SESSION_ENGINE = 'django.contrib.session.backends.db'
  
  # 以文件方式保存
  SESSION_ENGINE = 'django.contrib.session.backends.file'
  # 设置文件保存路径
  SESSION_FILE_PATH = '/MyDjango'
  
  # 以缓存形式保存
  SESSION_ENGINE = 'django.contrib.session.backends.cache'
  SESSION_CACHE_ALIAS = 'default'
  
  #设置浏览器Cookie中的键默认是sessionid，值是session_key
  SESSION_COOKIE_NAME='sessionid'
  ```

  





