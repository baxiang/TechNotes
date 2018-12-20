####安装版本django
```
pip install django
```
####创建项目
```
django-admin startproject demo1
```
项目目录结构
```
tree
.
├── demo1
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

1 directory, 5 files
```
manage.py是项目管理文件，通过它管理项目。
_init_.py是一个空文件，作用是这个目录demo1可以被当作包使用。
settings.py是项目的整体配置文件。
urls.py是项目的URL路由配置文件。
wsgi.py是项目与WSGI,服务器和Django交互的入口。
####创建应用
```
python manage.py startapp 名称
```
使用一个应用开发一个业务模块，此处创建应用名称为booktest，完成图书的信息维护。创建应用的命令如下：
```
python3 manage.py startapp books
```
目录结构
```
.
├── __init__.py
├── admin.py
├── apps.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py

1 directory, 7 files
```

_init.py_是一个空文件，表示当前目录books可以当作一个python包使用。
migrations 数据迁移模块
tests.py自动化测试模块。
models.py数据库操作相关模块。
views.py跟接收浏览器请求，进行处理，返回页面相关模块。
admin.py应用的的后台管理系统配置。
apps.py 版本是1.9以后的Django自动生成的。
####设置创建的应用
应用创建成功后，需要安装才可以使用，也就是建立应用和项目之间的关联，在demo/settings.py中INSTALLED_APPS下添加应用的名称就可以完成安装。
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'books'
]
```
####运行项目
运行python3 manage.py runserver 默认端口是8000,如果想修改端口号python3 manage.py runserver 8080，这样我们的端口就会变成了8080
```
python3 manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).

You have 15 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

November 21, 2018 - 16:36:26
Django version 2.1.3, using settings 'demo.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
[21/Nov/2018 16:36:35] "GET / HTTP/1.1" 200 16348
[21/Nov/2018 16:36:36] "GET /static/admin/css/fonts.css HTTP/1.1" 200 423
[21/Nov/2018 16:36:36] "GET /static/admin/fonts/Roboto-Regular-webfont.woff HTTP/1.1" 200 80304
[21/Nov/2018 16:36:36] "GET /static/admin/fonts/Roboto-Bold-webfont.woff HTTP/1.1" 200 82564
[21/Nov/2018 16:36:36] "GET /static/admin/fonts/Roboto-Light-webfont.woff HTTP/1.1" 200 81348
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-57e9a66f464f06c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
