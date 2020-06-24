---
layout: post
title: Flask AppBuilder
date: 2020-06-25
tags: Python Flask
---

Flask-AppBuilder

與安裝 Flask 一樣參考

https://www.echochio.nctu.me/2019/09/flask+uwsgi/

在 source env/bin/activate 後安裝相關套件
```
python -m pip install flask-appbuilder
```
或是 ...  有 requirements.txt ...

run.py 我改成這樣看一下是不是與 wsgi.py 很像

```
from app import app

app.run(host="0.0.0.0")

```
wsgi.py 這樣設會直接帶起flask-AppBuilder沒問題

```
from app import app as application

if __name__ == "__main__":
    application.run()
```

這邊就不紀錄 flask + uwsgi 的方式了

紀錄一下 root 測試環境

建立工作目錄
```
fabmanager create-app
```

改資料庫為 mysql 編輯 config.py 

```
SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://root:password@localhost/message'
```
其中 
```
帳號 : root
密碼 : password
主機 : locqlhost
資料庫 : message
```

建立 admin 帳密
```
fabmanager create-admin
```

執行
```
python3 run.py
```

這就不截圖了

改名 ... config.py 的 
```
APP_NAME = "短信APP"
```

修改首页 /app/index.py
```
from flask_appbuilder import IndexView
class MyIndexView(IndexView):
    index_template = 'new_index.html'
```

templates中new_index.html 改所想要的首页

修改app/__iniy__.py 在AppBuilder初始化的时候，指定indexview的值
```
from app.index import MyIndexView
appbuilder = AppBuilder(app, db.session, indexview=MyIndexView)
```

加 Helloword ... app/views.py
```
from flask_appbuilder import AppBuilder,expose,BaseView
class indexView(BaseView):
    # 相对路径的url
    route_base = '/index'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
appbuilder.add_view_no_menu(indexView())
```

只讓登入的看要加 @has_access
```
from flask_appbuilder import ModelView,AppBuilder,expose,BaseView,has_access
class indexView(BaseView):
    route_base = '/index'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
    @expose('/message/<string:msg>')
    @has_access
    def message(self,msg):
        msg = 'Hello' + msg
        return msg
appbuilder.add_view_no_menu(indexView())
```

app/templates/index.html
```
{% extends "appbuilder/base.html" %}
{% block content %}
    <h1>Welcome</h1>
    <h2>Hello World</h2>
    <h3>{{ msg }}</h3>
{% endblock %}
```

编辑/app/views.py,改成 .. 就可套用 templates/index.html
```
class indexView(BaseView):
    route_base = '/index'
    default_view = 'hello'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
    @expose('/message/<string:msg>')
    @has_access
    def message(self,msg):
        msg = 'Hello ' + msg
        return self.render_template('index.html',msg=msg)
# 在菜单中生成访问的链接
appbuilder.add_view(indexView,'Hello',category='My View')
appbuilder.add_link('message',href='/index/message/user',category='My View')
appbuilder.add_link('welcome',href='/index/hello',category='My View')
```

好了這邊紀錄一下 API 部分  app/views.py (這個與 flask 相近只比他寫在一個 class 就可)

要 import 那些就沒詳查了 ....
```
from flask import render_template
from flask_appbuilder.models.sqla.interface import SQLAInterface
from flask_appbuilder import ModelView, ModelRestApi
from . import appbuilder, db

from flask_appbuilder import ModelView,AppBuilder,expose,BaseView,has_access
from .models import Member,Message_list

from flask_appbuilder.api import BaseApi, expose
from flask import jsonify


class ApiView(BaseView):
    route_base = "/api"
    @expose("/list", methods=['GET'])
    def list(self):
        return jsonify({'res':'OK','apis':['/apiv1/devices','/apiv1/accesses','/apiv1/accounts']})

    @expose("/do/<p>", methods=['GET'])
    def alist(self,p):
        return jsonify({p:'OK'})

appbuilder.add_api(ApiView)
```

呼叫方式 
```
http://{domain}/api/{resource}/{params}
```

資料庫的部分 拿我亂寫的

app/models.py
```
from flask_appbuilder import Model
from sqlalchemy.dialects.mysql import LONGTEXT
from sqlalchemy.orm import relationship
from sqlalchemy import Column, Integer, String, DateTime, Text

class Member(Model):
    id = Column(Integer, primary_key=True)
    user =  Column(String(30), unique = True, nullable=False)
    nickname =  Column(String(50))
    user_password = Column(String(30))
    signature = Column(String(50))
    point = Column(Integer)
    account = Column(String(50))
    password = Column(String(50))
    extno = Column(String(50))
    def __repr__(self):
        return self.name

class Message_list(Model):
    id = Column(Integer, primary_key=True)
    message =  Column(Text)
    send_request =  Column(DateTime)
    returnlog = Column(LONGTEXT)
    point = Column(Integer)
    def __repr__(self):
        return self.message

```

app/views.py
```
from flask import render_template
from flask_appbuilder.models.sqla.interface import SQLAInterface
from flask_appbuilder import ModelView, ModelRestApi
from . import appbuilder, db

from flask_appbuilder import ModelView,AppBuilder,expose,BaseView,has_access
from .models import Member,Message_list

from flask_appbuilder.api import BaseApi, expose
from flask import jsonify

class MemberModelView(ModelView):
    datamodel = SQLAInterface(Member)
    #修改列名称
    label_columns = {'user':'帳號','nickname':'帳戶名稱','password':'密碼','signature':'客戶金鑰','point':'點數','account':'供應商帳號'}
    #定义列显示名称
    list_columns = ['user','nickname','password','signature','point','account'] #定义视图中要显示的字段
    show_fieldsets = [
        (
            'Summary',
            {'fields':['user','nickname','password','signature','point','account']}
        ),
        ]

class MessagelistModelView(ModelView):
    datamodel = SQLAInterface(Message_list)
    #修改列名称
    label_columns = {'message':'傳送資訊','send_request':'傳送時間','returnlog':'回應資訊','point':'剩餘點數'}
    #定义列显示名称
    list_columns = ['send_request','message','returnlog','point'] #定义视图中要显示的字段
    show_fieldsets = [
        (
            'Summary',
            {'fields':['send_request','message','returnlog','point']}
        ),
        ]

# 首先使用db.create_all()根据数据库模型创建表，然后再将视图添加到菜单。
db.create_all()
appbuilder.add_view(MemberModelView,'會員列表',icon = 'fa-address-card-o',category = '會員資訊')
appbuilder.add_view(MessagelistModelView,'紀錄列表',icon = 'fa-address-card-o',category = '紀錄')
```
