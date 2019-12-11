
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

    def __str__(self): #这个方法很有用，不仅可以在查看对象时使用，还能支持用来标记对象
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


## 涉及数据库的操作
```
  14 python manage.py migrate  # 执行具体的数据库操作。
  15 python manage.py makemigrations polls  #运行 makemigrations 命令，Django 会检测你对模型文件的修改（在这种情况下，你已经取得了新的），并且把修改的部分储存为一次 迁移。
  16 python manage.py sqlmigrate polls 0001 #sqlmigrate 命令接收一个迁移的名称，然后返回对应的 SQL
  17  python manage.py migrate #自动执行数据库迁移并同步管理你的数据库结构的命令
```
改变模型需要这三步：

* 编辑 models.py 文件，改变模型。
* 运行 python manage.py makemigrations 为模型的改变生成迁移文件。
* 运行 python manage.py migrate 来应用数据库迁移。

## 进入交互式 Python 命令行
通过` python manage.py shell`打开 Python 命令行 尝试一下 Django 为你创建的各种 API。

https://docs.djangoproject.com/zh-hans/2.2/intro/tutorial02/

```
from polls.models import Choice, Question #导入模型
Question.objects.all() #使用模型查询数据
from django.utils import timezone #导入需要的类
q = Question(question_text = "What is new?", pub_date = timezone.now()) #创建模型数据
q.save() #保存数据

Question.objects.filter(id=1)
Question.objects.filter(question_text__startswith='what')
Question.objects.get(pub_date__year=current_year)
Question.objects.filter(pk=1)
q = Question.objects.get(pk=1) #得到一个数据
q.choice_set.all() #得到question关联的choice
q.choice_set.create(choice_text='Not much', votes=0) #创建一个choice
c = q.choice_set.create(choice_text='Just hacking again', votes=0)
c.question
q.choice_set.all()
q.choice_set.count()
Choice.objects.filter(question__pub_date__year=current_year)
c = q.choice_set.filter(choice_text__startswith='Just hacking')
c.delete()
```

## 管理界面
管理界面不是为了网站的访问者，而是为管理者准备的。

```
python manage.py createsuperuser # 创建管理者页面
```

`admin created by zsp`

管理员登录界面：
```
http://127.0.0.1:8000/admin/
```

### 向管理页面中加入投票应用
只需要做一件事：我们得告诉管理页面，问题 Question 对象需要被管理。
打开 polls/admin.py 文件
```
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

### urlconf
`question_id=34` 由 `<int:question_id>` 匹配生成。使用尖括号“捕获”这部分 URL，且以关键字参数的形式发送给视图函数。

上述字符串的 `:question_id>` 部分定义了将被用于区分匹配模式的变量名，而 `int:` 则是一个转换器决定了应该以什么变量类型匹配这部分的 URL 路径。
```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)
 # ex: /polls/5/
path('<int:question_id>/', views.detail, name='detail'),
```

### 写一个真正有用的视图
每个视图必须要做的只有两件事：返回一个包含被请求页面内容的 HttpResponse 对象，或者抛出一个异常，比如 Http404 。

至于你还想干些什么，随便你。

Django 只要求返回的是一个 HttpResponse ，或者抛出一个异常。


### 返回一个HttpResponse
模板命名空间

虽然我们现在可以将模板文件直接放在 polls/templates 文件夹中（而不是再建立一个 polls 子文件夹），但是这样做不太好。

Django 将会选择第一个匹配的模板文件，如果你有一个模板文件正好和另一个应用中的某个模板文件重名，Django 没有办法 区分 它们。

我们需要帮助 Django 选择正确的模板，最简单的方法就是把他们放入各自的 命名空间 中，也就是把这些模板放入一个和 自身 应用重名的子文件夹里。


```
from django.http import HttpResponse
from django.template import loader
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {'latest_question_list': latest_question_list,}
    return HttpResponse(template.render(context, request))
    #快捷函数render():「载入模板，填充上下文，再返回由它生成的 HttpResponse 对象」是一个非常常用的操作流程。
    <!-- from django.shortcuts import render -->
    <!-- return render(request, 'polls/index.html', context) -->
```
载入 polls/index.html 模板文件，并且向它传递一个上下文(context)。这个上下文是一个字典，它将模板内的变量映射为 Python 对象。

### 抛出一个异常

* 抛出 404 错误
```
from django.http import Http404
try:
    question = Question.objects.get(pk=question_id)
except Question.DoesNotExist:
    raise Http404("Question does not exist")
```

尝试用 get() 函数获取一个对象，如果不存在就抛出 Http404 错误也是一个普遍的流程。
```
from django.shortcuts import get_object_or_404
question = get_object_or_404(Question, pk=question_id)
<!-- 也有 get_list_or_404() 函数 -->
```

### 去除模板中的硬编码 URL

```
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```
在urlconf中使用了name，所以可以进一步的替换来解耦，替换成这样
```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

### 为 URL 名称添加命名空间
多应用时存在同样的url名称时需要使用命名空间来区分

在根 URLconf 中添加命名空间。在`polls/urls.py` 文件中稍作修改，加上` app_name `设置命名空间：
```
app_name = 'polls'
```
使用
```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```
修改为指向具有命名空间的详细视图：
```
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```