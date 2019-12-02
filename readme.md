
## 新建专门用来存放项目的目录

`mkdir gjangogirls`

# 进入新建的项目目录中开始新的项目

* `cd .\gjangogirls\`
* `django-admin.exe startproject mysite .`
* 

### 项目下的文件作用
* manage.py 是一个帮助管理站点的脚本。在它的帮助下我们将能够在我们的计算机上启动一个 web 服务器，而无需安装任何东西。
* settings.py 文件包含的您的网站的配置数据。
* urls.py 文件包含urlresolver所需的模型的列表。

### 在 mysite/settings.py 更改设置
* 时区 `TIME_ZONE = 'Asia/Shanghai'`
* 静态文件的路径 =》在`STATIC_URL` 条目的下面新增行
```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```