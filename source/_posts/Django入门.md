---
title: Django入门
top: false
cover: false
author: DULULU oO
date: 2023-04-09 00:56:24
password:
summary:
tags:
    - Django
    - Python
categories: 基础知识
---


## 创建项目

进入虚拟环境 pip install django
打开anaconda，cd 到一个你想放置你代码的目录，输入：
```cmd
        django-admin startproject web
```
在该目录下会新建一个web项目存在
|web/
|── manage.py
|── web/
|       ├── __init__.py
|       | settings.py
|       | urls.py
|       | asgi.py
|       | wsgi.py

分别的用处：
这些目录和文件的用处是：

最外层的 **web/ 根目录**只是你项目的容器， 根目录名称对 Django 没有影响，你可以将它重命名为任何你喜欢的名称。
**manage.py**: 一个让你用各种方式管理 Django 项目的命令行工具。你可以阅读 django-admin 和 manage.py 获取所有 manage.py 的细节。
**里面一层的 web/ 目录**包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 myweb.urls).
**web/__init__.py：**一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。如果你是 Python 初学者，阅读官方文档中的 更多关于包的知识。
**web/settings.py：**Django 项目的配置文件。如果你想知道这个文件是如何工作的，请查看 Django 配置 了解细节。
**web/urls.py：**Django 项目的 URL 声明，就像你网站的“目录”。阅读 URL调度器 文档来获取更多关于 URL 的内容。
**web/asgi.py：**作为你的项目的运行在 ASGI 兼容的 Web 服务器上的入口。阅读 如何使用 ASGI 来部署 了解更多细节。
**web/wsgi.py：**作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。阅读 如何使用 WSGI 进行部署 了解更多细节。
在该目录下运行
```cmd
py manage.py runserver
```
在localhost:8000上部署成功
如果想要换端口，就输入py manage.py runserver+端口。
如果想要用ip地址访问（也称为远程访问），需要在8080前加上0.0.0.0:
也就是
```cmd
py manage.py runserver 0.0.0.0:8080
```

然后可以在浏览器输入本机ip：8080访问

## 创建应用


```cmd
python manage.py startapp myapp
```

myapp目录如下：

|web							项目根目录
├── myapp							应用名称
│   ├── migrations					数据模型迁移记录目录
│   │   └── __init__.py				inti文件，标识当前所在的数据模型迁移记录目录是一个 Python 包
│   ├── __init__.py					inti文件，标识当前所在的应用目录是一个 Python 包
│   ├── admin.py					Django Admin 应用的配置文件
│   ├── apps.py						应用程序本身的属性配置文件
│   ├── models.py					用于定义应用中所需要的数据表的配置文件
│   ├── tests.py					用于编写当前应用程序的单元测试的测试文件
│   └── views.py					用来定义视图处理函数的文件
├── web					项目名称
│   ├── __init__.py					
│   ├── settings.py					
│   ├── urls.py						
│   └── wsgi.py						
└── manage.py						

migrations 数据模型迁移记录目录：migrations 目录用于存储数据库迁移时生成的文件，该目录下的 __init__.py 文件标识 migrations 是一个 Python 包。

admin.py 文件：admin.py 用于将 Model 定义的数据表注册到管理后台，是 Django Admin 应用的配置文件；

apps.py 文件：apps.py 用于应用程序本身的属性配置文件；

models.py 文件：models.py 用于定义应用中所需要的数据表；

tests.py 文件：tests.py 用于编写当前应用程序的单元测试；

views.py 文件：views.py 用来定义视图处理函数的文件；

### 添加项目
在settings.py中

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',     # 添加应用
]
```