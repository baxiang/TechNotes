####ORM简介
对象关系映射（Object Relation Mapping）实现了关系和数据库之间的映射，隐藏了关系数据访问的细节，不需要再编写SQL语句
####创建模型
在models.py的文件中创建类 继承models.Model
```
from django.db import models


class BookInfo(models.Model):
    title = models.CharField(max_length=20)
    pub_date = models.DateField()
```
####数据迁移
1生成迁移文件：根据模型类生成创建表的迁移文件。执行命令python3 manage.py makemigrations APP名称，如果不指定APP的名称 会生成整个工程的数据迁移文件
```
python3 manage.py makemigrations
Migrations for 'books':
  books/migrations/0001_initial.py
    - Create model BookInfo
```
此时会在book文件下生成migrations/0001_initial.py迁移文件
```
from django.db import migrations, models


class Migration(migrations.Migration):

    initial = True

    dependencies = [
    ]

    operations = [
        migrations.CreateModel(
            name='BookInfo',
            fields=[
                ('id', models.AutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('title', models.CharField(max_length=20)),
                ('pub_date', models.DateField()),
            ],
        ),
    ]

```
2执行迁移：根据第一步生成的迁移文件在数据库中创建表。执行命令：python3 manage.py migrate
```
python3 manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, books, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying books.0001_initial... OK
  Applying sessions.0001_initial... OK
```
Django默认采用sqlite3数据库,最终会生成如下数据表，其中自定义的表命名规则是<app_name>_<model_name>（应用名称_模型类名）
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

```
![image.png](https://upload-images.jianshu.io/upload_images/143845-bed4703b049e0cb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####外键数据处理

```
class HeroInfo(models.Model):
    name = models.CharField(max_length=20)
    gender = models.BooleanField(default=False)
    comment = models.CharField(max_length=128)
    book = models.ForeignKey(BookInfo, on_delete=models.DO_NOTHING)

```
django 升级到2.0之后,表与表之间关联的时候,必须要写on_delete参数,否则会报异常:
TypeError: __init__() missing 1 required positional argument: 'on_delete'
```
on_delete参数的各个值的含义:

on_delete=None,               # 删除关联表中的数据时,当前表与其关联的field的行为
on_delete=models.CASCADE,     # 删除关联数据,与之关联也删除
on_delete=models.DO_NOTHING,  # 删除关联数据,什么也不做
on_delete=models.PROTECT,     # 删除关联数据,引发错误ProtectedError
# models.ForeignKey('关联表', on_delete=models.SET_NULL, blank=True, null=True)
on_delete=models.SET_NULL,    # 删除关联数据,与之关联的值设置为null（前提FK字段需要设置为可空,一对一同理）
# models.ForeignKey('关联表', on_delete=models.SET_DEFAULT, default='默认值')
on_delete=models.SET_DEFAULT, # 删除关联数据,与之关联的值设置为默认值（前提FK字段需要设置默认值,一对一同理）
on_delete=models.SET,         # 删除关联数据,
--------------------- 
```
