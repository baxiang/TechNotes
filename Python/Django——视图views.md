在books/view.py创建
```
from django.http import HttpResponse


def index(request):
    return HttpResponse('Hello Django')
```
在books创建urls.py 文件
```
from django.urls import path
from . import views
urlpatterns = [
    path('', views.index),
]
```
将books/urls.py引入到demo/urls.py文件中,在demo下的urls.py文件下，使用include()函数允许django引入其他url配置文件。
```
from django.contrib import admin
from django.urls import path,re_path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path('books/*', include('books.urls')),
```
#### 使用模板创建
在当前创建的app底下新建立templates文件夹，创建一个index.html的文件
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>Hello Django</h1>
</body>
</html>
```
返回当前的模板文件
```
from django.shortcuts import render


def index(request):
    return render(request, 'index.html')

```
####DTL的初步使用
render()函数中支持一个dict类型参数，该字典是后台传递到模板的参数，键为参数名，在模板中使用{{参数名}}来直接使用
修改index模板
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>Hello {{ name }}</h1>
</body>
</html>
```
在render函数中传递键值对
```
from django.shortcuts import render


def index(request):
    return render(request, 'index.html', {'name': 'Python'})

```
