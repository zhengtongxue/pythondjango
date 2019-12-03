
## 新建专门用来存放项目的目录

`mkdir gjangogirls`

# 进入新建的项目目录中开始新的项目

* `cd .\gjangogirls\`
* `django-admin.exe startproject mysite .`
* 

## 在新建的项目下新建应用程序来实现特定的目标完成专门的事情。
* `python manage.py startapp blog`
* 新增APP之后需要修改配置文件来使用它
修改`mysite/settings.py`文件，在` INSTALLED_APPS `增加`'blog',`。

一个项目Project下可以有多个应用程序App。


### 项目下的文件作用
* manage.py 是一个帮助管理站点的脚本。在它的帮助下我们将能够在我们的计算机上启动一个 web 服务器，而无需安装任何东西。
* settings.py 文件包含的您的网站的配置数据。
* urls.py 文件包含urlresolver所需的模型的列表。

### 在 mysite/settings.py 更改设置
* 时区 `TIME_ZONE = 'Asia/Shanghai'`
* 静态文件的路径 =》在`STATIC_URL` 条目的下面新增行，用来存放##静态文件和 CSS文件##。
```
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

* 设置数据库，使用sqlite3
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
新增完成之后在`manage.py`的同级目录下执行数据库操作`python manage.py migrate`，用来完成数据库的迁移

在以后涉及数据库的操作之后都应该执行该操作来升级迁移数据库的变更

* 开启 web 服务器运行服务
在`manage.py`的同级目录下执行数据库操作`python manage.py runserver`， 指定端口时额外添加 `0:8000`。


## 模型model
类：一类具有某些共性数据和操作的对象抽象到的类。

* 1、在`blog/models.py`文件中新增博客文章的模型model

```
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
* 2、在数据库中为模型创建数据表
在`manage.py`的同级目录下执行数据库操作`python manage.py makemigrations blog`来标记变更，之后再执行`python manage.py migrate blog`来迁移数据表。

* 3、在`Django admin` 管理后台导入新建的模型类
打开blog/admin.py文件
```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
使用`admin.site.register(Post)`来注册模型。

* 4、启动服务使用 `http://127.0.0.1:8000/admin/` 登录管理后台
需要使用`python manage.py createsuperuser`创建一个掌控整个网站所有东西的超级用户zsp^zsp。


## 部署PythonAnywhere 

PythonAnywhere 对于一些没有太多访问者的小应用是免费的，所以它对你来说绝对是足够使用的。
