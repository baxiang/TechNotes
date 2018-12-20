####设置项目根目录
```
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```
####本地化设置
语言设置为中文，时区设置为东八区
```
LANGUAGE_CODE = 'zh-hans'
TIME_ZONE = 'Asia/Shanghai'
```
####设置安装的APP
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
#### 设置管理员账号
```
python3 manage.py createsuperuser

```
####设置数据库查询
```
from django.contrib import admin
from books.models import BookInfo

admin.site.register(BookInfo)
```
####设置界面自定义显示
```
from django.contrib import admin
from books.models import BookInfo, HeroInfo


class BookInfoAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'pub_date']


class HeroInfoAdmin(admin.ModelAdmin):
    list_display = ['id', 'name', 'comment']


admin.site.register(BookInfo, BookInfoAdmin)
admin.site.register(HeroInfo, HeroInfoAdmin)
```
