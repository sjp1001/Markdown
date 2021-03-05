# FLASK

> flask牛逼就是牛在它本身是个轻量级web
>
> 如果你想用什么功能可以下载flask的关联库

***

## 基本用法

#### 基本流程

- 导入flask库

- 创建一个app应用

- 设定路由目录以及返回值

- 运行

  ```python
  from flask import Flask
  
  # 创建一个app应用
  # __name__指向程序所在的包
  app = Flask(__name__)
  
  # 装饰器的作用：将路由映射到视图函数
  # '/'是你网页的基础路径
  # 注：index是自己定义的函数！！！名字可以随便改
  @app.route('/')
  def intdex():
      return 'hello world'
  
  
  if __name__ == '__main__':
      # web服务器入口
      app.run()
  ```

#### 快速生成法（pycharm）

setting >> Editor >> Live Templates >> 找到flask >> 右键+号选第一个，自己设置模板

![](flask库\8.png)

1. 添加模板

2. 模板名

3. 跟2一致

4. 模板代码

5. 定位模板的使用位置（一定要设置）

6. 记得点ok

   ```python
   from flask import Flask
   
   app = Flask(__name__)
   
   @app.route('/')
   def intdex():
       return 'hello world'
   
   
   if __name__ == '__main__':
       app.run()
   ```

**以后输入flask_model就能会弹出上述代码**



## 基本配置

#### 初始化参数

```python
app = Flask(__name__)
# import_name 是Flask程序所在的包
# static_url_path 静态文件访问路径，可以不传。默认使用：static_folder
# static_folder 静态文件储存的文件夹，可以不传。默认使用：static
# template_folder 模板文件储存的文件夹，默认：templates
```

#### 配置对象

配置对象时候 里面可以定义需要给app添加的一系列配置（比如数据库链接，degub调试等）

> debug调试就是在设置网页时候不用反复开关

- 设置debug调试

  1. 从配置对象加载配置**（常用）**

     ```python
     class Condig(object):
         # 开启调试模式
         DEBUG = True
         
     app.config.from_object(Condig)
     ```
     
  2. 从配置文件加载配置

     先创建一个config.ini的配置文件，内容为  **DEBUG = True**

     ```python
   app.config.from_pyfile('config.ini')
     ```
     
  3. 从环境变量添加（pycharm可以自己设置环境中的值，很有意思）
  
     ![步骤一](flask库\1.png)

     ![步骤二](flask库\2.png)

     ![步骤三](flask库\3.png)

     ```python
   app.config.from_envvar('FLASKCONFIG')
     ```
  
  4. 在run函数直接设置（最简单，还能设置端口）
  
     ```python
     if __name__ == '__main__':
       app.run(debug = True,port = 8080)
     ```

#### 配置路由

- 路由参数传递

  ```python
  # <string> 可以在当前目录下，string可以读取传递
  @app.route('/demo/<string:user_id>')
  # string可以设定user_id的类型，如果输入int型则报错
  def intdex(user_id): # 将user_id传进来
      return '{} 学习很认真'.format(user_id) # 输出user_id
  ```

  ![](flask库\4.png)

- 设置请求方式

  ```python
  # 需要导入request库（flask库中就有）
  from flask import Flask,request
  
  @app.route('/demo2',methods = ['GET','POST'])
  def demo2():
      # 直接从请求中得到请求方式
      return request.method
  ```

  ![](flask库\5.png)

- 返回json数据

  ```python
  from flask import Flask,jsonify
  
  @app.route('/demo3')
  def demo3():
      json_dict = {
          'user_id' : 1,
          'user_name' : '嘉玮'
      }
      return jsonify(json_dict)
  ```

- 重定向

  ```python
  from flask import Flask,redirect
  
  # 重定向 （跳转到百度）
  @app.route('/demo4')
  def demo4():
      return redirect('https://www.baidu.com')
  ```

  ```python
  # 跳转到demo4再从demo4到百度
  @app.route('/demo5')
  def demo5():
      return redirect('/demo4')
  ```

  

- 设置状态码status code，让别人爬不到你的网站

  ```python
  @app.route('/demo6')
  def dem6():
      return '状态码在后边，随便设',666
  # 将状态码设置为666
  ```

  ![](flask库\6.png)



## 正则匹配路由

自己规定路由标准

eg.设置路由标准为0-9中的三个数

```python
from flask import Flask
from werkzeug.routing import BaseConverter

# 自定义转换器
class RegexConverter(BaseConverter):
    def __init__(self,url_map,*args):
        super().__init__(url_map)
        # 将第一个接受的参数当作匹配规则进行保存
        self.regex = args[0]
        
    def to_python(self, value):
        # 直接修改value
        return int(value)
        
app = Flask(__name__)

# 将自定义转换器添加到转换器字典中，并指定使用时的名字：re
app.url_map.converters['re'] = RegexConverter

# 设定re的标准0-9三个数中取3个数
@app.route('/user/<re("[0-9]{3}"):user_id>')
def index(user_id):
    return user_id

if __name__ == '__main__':
    app.run(debug = True)
```



## 异常捕获

- 先设定错误码

- 凡是这个错误码都显示指定内容

  ```python
  from flask import Flask
  
  app = Flask(__name__)
  
  @app.errorhandler(404)
  def server_error(e):
      return '服务器搬家了'
  
  if __name__ == '__main__':
      app.run()
      
  # 只要错误码是404就显示 '服务器搬家了'
  ```

  ![](flask库\7.png)



## 请求钩子

- 客户端 服务器 交互的时候 有些准备工作或者扫尾工作需要处理
- 在请求开始时，建立数据库链接
- 在请求开始时，根据需求进行权限验证
- 在请求结束时，指定数据的交互格式

> 就是设置在请求函数之前的操作，和请求函数之后的操作

```python
from flask import Flask,abort

app = Flask(__name__)

# 在第一次请求之前调用，可以在此方法内部做一些初始化操作
@app.before_first_request
def before_first_request_demo():
    print('before_first_request')

# 在每次请求之前调用
@app.before_request
def before_request_demo():
    print('before_request')

# 在执行视图函数之后调用，并且会把视图函数的响应传入 进行统一处理
@app.after_request
def after_request_demo():
    print('after_request')


if __name__ == '__main__':
    app.run()
```



## 状态保持

Cookie：指某些网站为了辨别用户身份 进行会话跟踪而储存用户在本地的数据

储存在浏览器当中 一段纯文本信息 不同的域名 Cookie是不能相互访问的

```python
from flask import Flask,make_response

app = Flask(__name__)

@app.route('/cookie')
def intdex():
    resp = make_response('这是Cookie')
    # 设置cookie的用户名，以及过期时间
    resp.set_cookie('username','jiawei',max_age = 3600)
    return resp

if __name__ == '__main__':
    app.run()
```

![](flask库\9.png)

#### session会话

session依赖于cookie，在服务器保持session

**（但是老师没讲明白，后续用到再补充吧）**



##  jianjia2模板



## WTF表单模板



## csrf安全



## MYSQL数据库

#### 安装需要的库

- `pip install flask-sqlalchemy`

  SQLAlchemy是一个关系型数据库框架，它提供了高层的 ORM 和底层的原生数据库的操作。flask-sqlalchemy 是一个简化了 SQLAlchemy 操作的flask扩展。

- `pip install flask-mysqldb`

  如果连接的是 mysql 数据库，需要安装 mysqldb

#### 使用pycharm链接mysql

![](flask库\10.png)

![](flask库\11.png)

1. 数据库用户名

2. 数据库密码

3. 数据库端口号（以及地址）全在上边设置完了

4. 下载缺失的驱动文件，可以上来就点

   > 还有其他的比如Database的东西也可以设置

**最后点击 Test Connection 测试链接****

![](flask库\12.png)

1. mysql操作面板
2. 写完mysql语句，点击运行

#### SQLAlchemy库

通过建类的方法建表

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# 设置数据库的链接
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:root@127.0.0.1：3306：text_t'

# 动态追踪修改设置
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True

# 查询显示原生的SQL语句
app.config['SQLALCHEMY_ECHO'] = True

# 数据库进行关联
db = SQLAlchemy(app)


# 创建一个表的模板（后边需要用的时候直接拿类生成就好）
class Role(db.Model):
    # 定义表名
    __tablename__ = 'role'
    # 主键
    id = db.Column(db.Integer,primary_key=True)
    # 不重复
    name = db.Column(db.String(64),unique=True)
    us = db.relationship('User',backref='role')

    def __repr__(self):
        return 'Role:%s' % self.name
    
if __name__ == '__main__':
    # 增删改查用法（百度吧）
    pass
```