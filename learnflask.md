##Flask学习笔记

####简单的包
- 对于更大的应用而言，使用包来代替模块是一个不错的选择，要创建一个包，应该有如下的目录结构：
```
---/app
-----__init__.py
----templates/
----static/
----views.py
---run.py
```
- 如何让应用跑起来呢？？run.py
```
from app import app
app.run(debug=True)
# python run.py
```
- Flask应用的对象(app)必须放在\_\_init\_\_.py文件内。所有view方法（包含有@app）必须放在\_\_init\_\_py
里import
```
from flask import Flask
app = Flask(__name__)
from app import views

#views.py的例子
from app import app
@app.route('/')
def index():
	pass
```
####使用Sqlite3
- 使用before_request()在请求开始时，打开连接。使用teardown_request()在结束时，关闭连接。
```
import sqlite3
from flask import Flask

DATABASE = '/tmp'

def connect_db():
	return sqlite3.connect(DATABASE)

@app.before_request
def before_request():
	g.db = connect_db()
@app.teardown_request
def teardown_request(execption):
	g.db.close()

# 另外也可以使用after_request
@app.after_request
def after_request(response):
	g.db.close()
	return response
```
- 在每个请求中，可以使用g.db来得到打来的数据库连接
```
def query_db(query, args=(), one=False):
	cur = g.db.execute(query, args)
	rv = [dict((cur.description[idx][0], value)
			for idx, value in enumerate(row)) for row in cur.fetchall()]
	return (rv[0] if rv else None) if one else rv

for user in query_db('select * from users')

#other

users = g.db.execute('select * from usres where name = ?', [the_username], one=Ture)
```
- 用sql脚本来初始化数据库
```
from contextlib import closing

def init_db():
	with closing(connect_db()) as db:
		with app.open_resource('schema.sql') as f:
			db.cursor().ececutescript(f.read())
		db.commit()

from app import init_db
init_db()
```		

####模板继承

- base template
```
<html>
	<head>
		{% block head %}
		<title>{% block tilte %}{% endblock %}</title>
		{% endblock%}
 	</head>
	<body>
		<div>
			{% block content %}{% endblock %}
		</div>
		<div>
			{% block footer %}{% endblock %}
		</div>
	</body>
</html>
```
- {%block%}定义的标签就是就是子模板能添的地方。
```
{% extends "base.html" %}
{% block title %}Index{% endblock %}
{% block head %}
{{ super() }}
{% endblock %}
{% block content %}
<h1>Index</h1>
{% endblock %}
```
- {% extends %}告诉子模板继承其他的模板.要render定义在模板中的block的内容。使用{{ super() }}
