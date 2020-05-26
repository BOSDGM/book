# 1. flask-sqlalchemy

## 1.1 helloword

```python
app.config.from_object(Config)
db = SQLAlchemy(app)


class Number(db.Model):
    __tablename__ = "number_test"
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))

    def __repr__(self):
        return 'class:%s' % self.name

@app.route("/a")
def query():
    page = int(request.args.get("page", 1))
    ns = Number.query.order_by(Number.id.desc()).paginate(page=page, per_page=3).items
    return str([n.name for n in ns])
```



## 1.2 配置文件

```python
# 数据库配置
    # mysql+pymysql://username:password@127.0.0.1:3306/test
	# postgresql://scott:tiger@localhost/mydatabase
	# - oracle://scott:tiger@127.0.0.1:1521/sidname
	# sqlite:////absolute/path/to/foo.db
SQLALCHEMY_DATABASE_URI = "mysql://username:password@ip:port/db_name"
# 绑定多个数据库
SQLALCHEMY_BINDS = {
	"db1": "mysql://username:password@ip:port/db_name",
    "db2": "sqlite:////path/to/appmeta.db"
}
# 是否开启查询语句显示
SQLALCHEMY_ECHO=False
# 是否读取每一句查询语句的具体信息(利用flask_sqlalchemy.get_debug_queries()语句/执行时长等)
SQLALCHEMY_RECORD_QUERIES=False # 测试/debug=True默认自动打开的
# 是否禁用UNICODE编码
SQLALCHEMY_NATIVE_UNICODE=False
# 数据库连接池
SQLALCHEMY_POOL_SIZE=5
# 数据库连接池超时(s)
SQLALCHEMY_POOL_TIMEOUT=10
# 数据库连接收回(s)
SQLALCHEMY_POOL_RECYCLE=2 * 60 * 60
# 超出连接池最大值可创建的额外连接数, 使用后将会被抛弃
SQLALCHEMY_MAX_OVERFLOW
# 是否关闭信号追踪:
	# https://blog.csdn.net/weixin_42225318/article/details/80984198
	注: flask提供了信号追踪的接口(flask.signals最后几行), 需要先安装pip install blinker
    用途: 
        1. 检测各种请求钩子的调用, 并返回调用对象处理结果
        2. sqlalchemy对信号支持, 检测数据库的增删改操作, 并返回操作对象和list[(操作对象, "insert/delete/update")]
        3. 关闭可以提高服务器性能
        4. 对两个装饰函数有用:
            before_models_committed/models_committed
SQLALCHEMY_TRACK_MODIFICATIONS=False
```

# 2. 字段

# 3. 数据库信号

## 3.1 执行前

```python
@models_committed.connect_via(app)
def models_committed(a, changes):
    """对数据库的增删该操作进行捕获处理"""
    print(a is app, changes)
```

## 3.2 执行后

```python
@before_models_committed.connect_via(app)
def models_committed(a, changes):
    """两个数据一模一样, 一个是操作后的, 一个是操作前的"""
    print(a is app, changes)
```

## 3.3 获取慢查询

```python
@app.after_request
def after_request(response):
    """在每次查询后, 获取慢查询记录"""
    for query in get_debug_queries():
        # if query.duration >= 0.001:  # 用时判断
        print(
                ('\nContext:{}\nSLOW QUERY: {}\nParameters: {}\n'
                 'Duration: {}\n').format(query.context, query.statement,
                                          query.parameters, query.duration))
    return response

```



## 3.4 测试代码

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
# author:hpcm
# datetime:19-6-17 下午4:32
from flask import Flask, request
from flask_migrate import Migrate, MigrateCommand
from flask_script import Manager
from flask_sqlalchemy import SQLAlchemy, models_committed, before_models_committed

app = Flask(__name__)


class Config(object):
    # 全局配置
    SECRET_KEY = "hello_world"
    DEBUG = False
    # mysql配置
    SQLALCHEMY_DATABASE_URI = "mysql+pymysql://root:dong10@localhost:3306/db_test"
    SQLALCHEMY_TRACK_MODIFICATIONS = True
    SQLALCHEMY_ECHO = False
    SQLALCHEMY_RECORD_QUERIES = False


app.config.from_object(Config)
db = SQLAlchemy(app)


class Number(db.Model):
    __tablename__ = "number_test"
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))

    def __repr__(self):
        return 'class:%s' % self.name

    def to_dict(self):
        return {
            "id": self.id,
            "name": self.name
        }


@app.route("/a")
def query():
    page = int(request.args.get("page", 1))
    ns = Number.query.order_by(Number.id.desc()).paginate(page=page, per_page=3).items
    return str([n.name for n in ns])


@app.route("/b")
def add_one():
    n1 = Number()
    n1.name = "test_{}".format("111")
    n2 = Number()
    n2.name = "test_{}".format("111")
    db.session.add_all([n1, n2])
    db.session.commit()
    return "success!"


@models_committed.connect_via(app)
def models_committed(a, changes):
    """对数据库的增删该操作进行捕获处理"""
    print(a is app, changes)


@before_models_committed.connect_via(app)
def models_committed(a, changes):
    """两个数据一模一样, 一个是操作后的, 一个是操作前的"""
    print(a is app, changes)
    
    
@app.after_request
def after_request(response):
    """在每次查询后, 获取慢查询记录"""
    for query in get_debug_queries():
        # if query.duration >= 0.001:  # 用时判断
        print(
                ('\nContext:{}\nSLOW QUERY: {}\nParameters: {}\n'
                 'Duration: {}\n').format(query.context, query.statement,
                                          query.parameters, query.duration))
    return response


migrate = Migrate(app, db)

manager = Manager(app)
manager.add_command("db", MigrateCommand)

if __name__ == "__main__":
    manager.run()
```





