---
title: "Django快速入门"
categories: code
tags: ['Django']
id: "887a8ba9693f6ba0"
date: 2025-04-29 15:22:31
cover: "https://wp-cdn.4ce.cn/v2/AXCnzEa.jpeg"
---

:::note
Django快速入门
:::

# DjangoWeb开发

## Day1

### 1. Django的安装

```
pip install django
```

### 2. 创建项目

#### 2.1 终端创建

```
“python环境路径\scripts\django-admin.exe” startproject django项目名
# 如果python环境路径配置了环境变量，可直接写
django-admin startproject django项目名
```

#### 2.2 pycharm创建

#### 2.3 项目文件介绍

```
django_study_demo
│─ manage.py			【项目管理的脚本，不要修改，eg：启动、创建app、数据库管理等】
└─django_study_demo		【与项目同名的文件夹】
     │─ asgi.py			【和wsgi.py一起，接收网络请求的】【不用修改】【Django接收异步的】
     │─ settings.py		【项目的配置文件，eg：数据库连接信息、注册app等】【常操作】
     │─ urls.py			【全部的URL和函数的对应关系】【常操作】
     │─ wsgi.py			【和asgi.py一起，接收网络请求的】【不用修改】【Django接收同步的】
     │─ __init__.py

```

### 3. 创建app

#### 3.1 Pycharm命令行创建

```
python manage.py startapp app名
```

#### 3.2 app目录介绍

```
django_study_demo
│  manage.py
├─django_study_demo
│     asgi.py
│     settings.py
│     urls.py
│     wsgi.py
│     __init__.py
└─index						【app名命名的文件夹】
    │  admin.py				【固定的不用动】django默认提供的后台管理，但实际开发不常用
    │  apps.py				【固定的不用动】app启动相关
    │  models.py			【☆很重要，对数据库进行操作】这里不用SQL写了，Django封装了ORM供调用
    │  tests.py				【固定的不用动】用来单元功能测试的，个人小项目可以不用管
    │  views.py				【☆很重要，撰写视图函数（得我们自己写的）】
    │  __init__.py
    └─migrations			【固定的不用动】数据库变更记录，会自动生成文件，我们不用动
            __init__.py

```

### 4. 快速上手

#### 4.1 确保app已注册

1. 在settings.py的列表变量INSTALLED_APPS中，添加一个 'app名' ，添加新app

#### 4.2 URL和函数的映射

1. 找到urls.py中列表变量urlpatterns , 添加一行path(' ' , )

    ```
    path('', include(("index.urls", 'index'), namespace='index')),
    ```

2. path(param1 , param2 , param3 )需要三个参数

    ```python
    param1 : URL
    param2 : views.py的视图函数
    param3 : 命名空间，方便后续模板层调用
    ```

#### 4.3 视图函数的撰写

修改相应app下的views.py

注意点：

```
1.django中的视图函数必须带request参数
2.视图函数必须return，一般返回的都是要传给前端的数据
```

```python
def index(request):
	return HttpResponse("hello, World")
```

#### 4.4 运行Django

命令行启动django项目：

```
python manage.py runserver 127.0.0.1:8008
```

#### 4.5 创建页面

如果还要创建新的页面，只需要两个步骤：

1. views.py中撰写相应函数
2. urls.py中定义URL和函数的对应关系

```python
# 增加部分
# urls.py
path('user/add/', views.user_add),
path('user/delete/', views.user_del),

# views.py
def user_add(request):
    return HttpResponse("用户添加")

def user_del(request):
    return HttpResponse("用户删除")

```

#### 4.6 模板文件&静态文件&媒体文件

##### 4.6.1 render()使用

```python
def user_list(request):
	# param1：request参数 ， param2：html文件路径
	return render(request,"user_list.html")
```

##### 4.6.2 模板路径问题

1. 找到settings.py中TEMPLATES列表变量，修改"DIRS": [BASE_DIR/'templates']

    ```python
    #配置模板路径
    TEMPLATES = [
        {
            "BACKEND": "django.template.backends.django.DjangoTemplates",
            "DIRS": [BASE_DIR/'templates'],
            "APP_DIRS": True,
            "OPTIONS": {
                "context_processors": [
                    "django.template.context_processors.debug",
                    "django.template.context_processors.request",
                    "django.contrib.auth.context_processors.auth",
                    "django.contrib.messages.context_processors.messages",
                ],
            },
        },
    ]
    ```

2. 在django项目根目录创建templates文件夹

##### 4.6.3 静态文件static

1. 找到settings.py，配置静态文件

    ```python
    #配置静态文件
    STATIC_URL = "/static/"
    STATICFILES_DIRS = (
        os.path.join(BASE_DIR, 'pstatic'),
    )
    ```

2. 在django项目根目录创建pstatic文件夹

3. 在pstatic文件夹下创建img、css、js、plugins(插件：如Bootstrap：前端开发CSS框架)文件夹存放对应内容

注意点总结：

- 声明static语法：{% load  static %} 要写在顶部，写在html文件开头
- 声明static的原理：
    1. 声明static会从settings.py中的STATIC_URL去找
- 调用语法：{% static '...' %}，其中 ... 是静态文件的路径
- 用{% ... %}的优点：
    1. 比较规范(符合Django模板开发规范)
    2. 修改static文件路径时，方便修改(只需要改settings.py即可)

##### 4.6.4 媒体文件

1. 在settings.py 中配置媒体文件

    ```python
    #配置媒体文件
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```

2. 在django项目根目录中创建media文件夹

3. 在目路由中配置media路由

    ```python
    # 配置媒体资源的路由信息
    re_path('media/(?P<path>.*)', serve, {'document_root': 	       settings.MEDIA_ROOT},name='media')
    ```

4. 运行django项目，可通过url访问媒体资源 测试

### 5. Django模板语法

本质：在html中写一些占位符，然后由数据对这些占位符进行替换和处理

PS：上面提到的占位符 static 就是和 /static/ (settings.py中STATIC_URL 的值) 做了替换

#### 5.1 调用后端数据

1. render()函数 --> param3：向模板层传递数据，不过**必须是字典**
2. 模板层 --> 通过{{ ... }}的语法根据键名来调用相应的数据

#### 5.2 索引

对于列表数据，通过{{ 列表名.索引 }} 获取数据（ 和python不同 , python是通过 [] ）

#### 5.3 循环语句

语法：

```django
{% for i in ... %}
	...
{% endfor %}
```

- 对于列表数据:
    - 循环使用即可

- 对于字典数据:
    - 跟python一样，有keys、values、items
    - 唯一不同：for foo in user_info.keys() -->不要带括号

#### 5.4 条件语句

```django
{% if name == "XXX" %}
	...
{% elif name == "XXX" %}
	...
{% else %}
	...
{% endif %}
```

和python一样，就是不用加 :  , 结束必须带个 {% endif %}，其他都一样

#### 5.5 模板语法原理

原理：

1. render( ) 读取带有模板语法的html文件
2. django内部进行渲染 ( 模板语法执行并替换成数据 )
3. 最终得到只包含html标签的字符串
4. 将渲染完成的字符串传给用户浏览器

### 6. Request & Response

即请求和响应

#### 6.1 Request

request就是一个对象，封装了用户通过浏览器（爬虫等许多途径）发送过来的所有请求相关的数据，常用的有：

- request.method  : 获取用户请求提交方式（GET/POST）
- request.GET : 获取通过URL传递的参数
- request.POST : 通过请求体获得数据

#### 6.2 Response

视图函数中return所返回的，就是我们要写的response，常用的有：

- HttpResponse( ) : 返回内容字符串给请求者

- render ( ) : 读取HTML，并渲染，最终以字符串的格式返回给用户浏览器

- redirect( ) : 让浏览器重定向到其他页面，返回的是网址

    ```
    Q: 重定向这个过程是怎么样的?
    A: 浏览器提交请求给Django，Django收到请求并直接返回给浏览器重定向的网址，浏览器重新去新的网址那里提交请求，然后新的网址给浏览器返回响应数据
    ```

#### 6.3 练习

做一个简单的登录功能：

1. 先简单做一个登录页面

2. 设置路由、视图函数、以及映射关系，访问相应URL测试

3. 设置form表单的提交地址是触发视图函数login( ) , 而且采用post请求

4. 撰写login( ) 中的代码逻辑

    ```python
    def login(request):
    	if request.method == "GET":
    		return render(request,"login.html")
    	elif request.method == "POST":
    		post_data = request.POST
    		print(post_data)
    		return redirect("https://www.baidu.com")
    ```

    PS : 浏览器一开始访问login.html（GET请求），视图函数login( )就render渲染login.html；填写完 form表单点击提交，再次访问就是POST请求了

5. 测试功能

    ```
    这段代码复刻到flask是没有问题的，在django中会报错，因为django自带了一个csrf，一个安全机制
    解决方法：在form表单中，添加一行占位符：{% csrf_token %}
    ```

    PS : django内部对其渲染时，将其渲染为一个隐藏输入框，并提交一个csrfmiddlewaretoken参数，值为一个列表，列表中有一个值：自动生成

### 7. 数据库操作 ORM

ORM：封装了连接数据库的语句和SQL语句( 不用自己去写 )，而且增加了安全机制

#### 7.1 安装第三方模块

新版本的django默认采用mysqlclient这个库

```
pip install mysqlclient
```

#### 7.2 ORM

ORM可以帮我们做两件事：

- 增删改查数据库中的表（不用写SQL语句）【但数据库得自己建】
- 操作表中数据，即增删改查表中记录（不用写SQL语句）

##### 7.2.1 创建数据库

- 启动MySQL服务（默认都是开的）
- 创建数据库

##### 7.2.2 django连接数据库

django连接数据库只需要在 settings.py中 DATABASES字典 配置连接参数即可

```python
#配置数据库
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": 'stock_allocation_system',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

##### 7.2.3django操作表

- 创建表
- 删除表
- 修改表

**增加表**

在相应app中的models.py中写一个类就是创建一张表

```python
# 在MySQL中创建对应的一个表
class UserInfo(models.Model):
	# 创建字段
	name = models.CharField(max_length=32)
	password = models.CharField(max_length=64)
	age = models.IntegerField()
```

ORM会帮我们把这个面向对象的代码转化为SQL（表名是app名称_类名）

```mysql
create table index_userinfo(
    id bigint auto_increment primary key,		【django创建表自带自增字段】
    name varchar(32),
    password varchar(64),
    age int,
);

```

到terminal界面下执行命令，通过manage.py来让ORM读取models.py中的代码

```
python manage.py makemigrations
python manage.py migrate
```

**删除表**

把 models,py中类注释掉，再执行那两个指令即可删除表

**修改表**

增加字段：涉及到已存在数据的新增字段

所以存在三种增加字段方式：

- 不给默认值，terminate给默认值
- 给默认值
- 允许为空

删除字段、修改字段：直接更改，然后执行那两个指令

**表内增删改查**

**增**

语法：类名.objects.create( )

```python
# insert into index_studentinfo(title) values("zm");
StudentInfo.objects.create(title="zm")
# insert into index_userinfo(name,password,age) values("gzh","123",18);
UserInfo.objects.create(name="gzh", password="123", age=18)
```

**删**

语法：

- 类名.objects.filter( ).delete( )：筛选内容，再删除
- 类名.objects.all( ).delete( )：删除表内全部内容

```python
UserInfo.objects.filter(id=2).delete()	# 删除表中id为2的删除
StudentInfo.objects.all().delete()		# 删除表中所有内容
```

**改**

语法：

- 类型.objects( ).all( ).update( )：修改全部
- 类型.objects( ).filter( ).update( )：筛选内容，再修改

```python
UserInfo.objects().all().update(password="123")		# 把表中password全改成123
UserInfo.objects().filter(id=2).update(password="1")	# 把表中id为2的那行数据的password改成1
```

**查**

语法：

- 类名.objects.all( )：查询表中所有数据
- 类名.objects.filter( )：筛查相应的数据

注意：返回的数据都是一个QuerySet类型数据，可以理解为List，只是每一个元素是一个对象罢了

```python
all_data = UserInfo.objects.all()
for obj in all_data:
    print(obj.id, obj.name, obj.password, obj.age)
```

```python
# UserInfo.objects.filter(id=1)		# 获得一行数据，但是也是QuerySet的一个列表，只是只有一个元素
# 所以可以通过first()来取到第一个单独的元素
data = UserInfo.objects.filter(id=1).first()
print(data.id, data.name, data.password, data.age)
```

## Day2

管理系统：员工管理系统

### 1. 创建项目

1. 在terminate创建项目
2. 配置模板文件
3. 配置静态文件
4. 配置媒体文件
5. 配置数据库

### 2. 创建app

1. 可通过菜单栏工具选项中运行 manage.py任务（省略掉了 python manage.py）
2. 注册app

### 3. ORM创建表结构

#### 3.1 常见参数位：

整理了一些model中字段名的常见参数：

- max_length：一般是CharField( )中用，字符的最大长度
- verbose_name：注释字段名，如果调用django的admin模块来后台管理的话，字段名会自动替换成verbose_name
- primary_key：声明主键，唯一且非空，一般都是写primary_key=True , =False的话不写即可
- default：默认值
- max_digits：DecimalField( )用，规定数字最长是多少（不包括正负号）
- decimal_places：DecimalField( )用，规定显示小数点后几位
- null & blank：声明该字段可以为空，组合使用，一般都是写null=True，blank=True，=False的话不写即可
- choices：添加django的约束，给这个参数赋值时得用元组

#### 3.2 创建表结构

#### 3.3 设置表间关系

##### 3.3.1 一对多 ForeignKey

```PYTHON
dep = models.ForeignKey(to="Department", to_field="id")
PS：生成表后，django会自动在此字段名后加上_id，所以实际表中字段名是dep_id
```

外键即 关系的约束，需设置删除数据时的解决方法：

- 删除关联的员工：级联删除，相应配置为on_delete  = models.CASCADE
- 不删置为空：这样写，必须设置允许该字段可以为空，相应配置为null=True，blank=True，on_delete=models.SET_NULL

##### 3.3.2 添加django约束

上面所说为数据库的约束，django也可以添加约束

```python
# 添加django中的约束
gender_choices = (
	(1,"男"),
	(2,"女")
)
注意：必须得是元组
gender = models.SmallIntegerField(verbose_name="员工性别",choices=gender_choices)
```

#### 3.4 创建数据库

#### 3.5 指令生成

#### 6. 部门管理

接下来开始功能实现

##### 6.1 原始方法实现

###### 6.1.1 部门列表

**三要素：**

创建dep_list.html，撰写视图函数dep_list( )，设置子母路由

```python
def dep_list(request):
	"""部门列表"""
	
	# 获取数据库中数据
	from em_web.models import Department
	all_dep_data = Department.objects.all()  # QuerySet(<对象>, <对象>, ...)
	return render(request,"dep_list.html",{"dep_queryset":all_dep_data})
```

###### 6.1.2 部门添加

**三要素：**

创建dep_add.html，撰写视图函数dep_add( )，设置子母路由

###### 6.1.3 部门删除

**三要素：**

创建dep_del.html，撰写视图函数dep_del( )，设置子母路由

###### 6.1.4 部门编辑

**三要素：**

创建dep_updata.html，撰写视图函数dep_update( )，设置子母路由

1. 设置子路由传参问题

    ```python
    # 访问连接"127.0.0.1:8008/dep/数字/update/时触发"
    path('dep/<int:nid>/update/',views.dep_update)
    ```

2. 视图函数dep_update( )

    ```python
    def dep_update(request,nid):
    	if request.method == 'POST':
    		new_dep_name = request.POST.get("new_dep_name")
    		Department.objects.filter(id=nid).update(dep_name=new_dep_name)
    		return redirect("/dep_list/")
    	default_dep = Department.objects.filter(id=nid).first()
    	return render(request,"dep_update.html",{
    		"default_dep":default_dep
    	})
    ```

###### 6.1.5 模板继承

将重复的模板代码单独写到一个 base.html 中，供其他html文件所调用

语法：首先将需要继承的部分不动，把不需要继承的地方换成这段代码：

```django
{# XXX时这个块的名字，子模版想要往哪加代码，就是通过名字来确定的}

{% block XXX %}
	...
{% endblock %}
```

模板继承的好处：

- 可以定义css块：子模版引入只有自己使用的样式
- 定义js块：子模版引入只有自己使用的js
- 修改时只需要修改模板即可

**语法总结：**

```django
{# 定义块 #}
{% block XXX %}{% endblock %}

{# 继承语法 #}
{# 要写前面，先继承，再调用块 #}
{% extends 'XXX.html' %}
{# 调用块，并写入代码 #}
{% block XXX %}
	...
{% endblock %}
```

###### 6.1.6 员工管理

重要点：

- 员工入职时间（处理特殊数据类型）可通过strftime("%Y-%m-%d-%H-%M")将datetime类型转换成string类型
- 员工性别（django约束 )不能显示1和2，得用django的方法来调用，调用方法是get_字段名称\_display( )
- 员工所属部门（外键），emp.dep，django会把名为dep的那个外键，自动链表，返回外表中相应的那行数据
    - PS：外键叫啥就写啥，不用加_id，所以应写emp.dep.dep_name

对数据做处理：

- 后端处理，传到前端渲染

- 后端不处理，传到前端，前端处理

    ```python
    python语法和django模板语法不同：
    模板语法中：
    	不允许加(),需要加括号的，去掉就好;
    	括号里带参数的，用|写，而且django模板改写了strftime(),封装成了date
    所以：
    {{ emp.create_time|date:"Y-m-d H:i:s" }}
    ```

##### 6.2 Form&ModelForm

django提供了Form组件和ModelForm组件，Form组件是比较简便的，ModelFrom组件是最简便的

###### 6.2.1 Form概述

```python
# views.py
class MyForm(Form):
    {# widget=forms.Input表示在前端渲染成输入框 #}
    user = forms.CharField(widget=forms.Input)
    pwd = forms.CharField(widget=forms.Input)
    email = forms.CharField(widget=forms.Input)

def emp_add(request):
    """员工新建"""
    if request.method == "GET":
        form = MyForm()
        return render(request, "emp_add.html", {"form": form})
```

```django
{# emp_add.html #}
<form method="post">
    {{ form.user }}
    {{ form.pwd }}
    {{ form.email }}
</form>

{# 或者这么写 #}
</form>
    {% for field in form %}
        {{ field }}
    {% endfor %}
</form>
```

可以发现，MyForm里面的内容和models.py中数据表的代码很像，所以我们可以用ModelForm组件更简化代码。

###### 6.2.2 ModelForm概述

```python
# models.py
class Employee(models.Model):
    """员工表"""
    name = models.CharField(verbose_name="员工姓名", max_length=16)
    password = models.CharField(verbose_name="员工密码", max_length=64)
    age = models.IntegerField(verbose_name="员工年龄")
    account = models.DecimalField(verbose_name="账户余额", max_digits=10, decimal_places=2, default=0)
    create_time = models.DateTimeField(verbose_name="入职时间", )

    # 添加django中的约束
    gender_choices = (
        (1, "男"),
        (2, "女"),
    )
    gender = models.SmallIntegerField(verbose_name="员工性别", choices=gender_choices)

    # 生成表时，django会生成dep_id字段而不是dep字段
    dep = models.ForeignKey(verbose_name="所属部门", to="Department", to_field="id", on_delete=models.CASCADE)

```

```python
# views.py
class MyForm(ModelForm):
	class Meta:
        model = Employee
        # 想加什么字段，放在列表中即可
        fields = ["name", "password", "age"]

def emp_add(request):
    """员工新建"""
    if request.method == "GET":
        form = MyForm()
        return render(request, "emp_add.html", {"form": form})

```

```python
{# emp_add.html #}
</form>
    {% for field in form %}
        {{ field }}
    {% endfor %}
</form>

```

ModelForm对于那些操作数据表增删改查的功能，是最简便的方法，使用也不止上面这些方法，我们只是简单说一说。下面我们边实现功能，边学习ModelForm的更多使用方法。

###### 6.2.3 员工新建（ModelForm实现）

注意点：

- 在前端可通过 .label获取到models.py中对应verbose_name的值，用于输入框前面的文本渲染

- 输入框可自主添加属性

    ```python
    class EmpAddForm(forms.ModelForm):
    	"""员工新建的ModelForm"""
    	class Meta:
    		model = Employee
    		fields = ['name','password','age','account','create_time','gender','dep']
    	
    	def __init__(self,*args,**kwargs):
    		super().__init__(*args,**kwargs):
    		# 循环找到所有的插件，然后添加样式
    		for name,field in self.fields.items():
    			# 如果某些输入框的样式不想调整，可以增加判断
    			if name == "password":
    				continue
    			field.widget.attrs = {"class":"form-control","placeholder0:field.label"}
    			
    ```

- 实现post请求（基于ModelForm的方法）

    ```python
    def emp_add(request):
    	"""基于ModelForm实现的员工新建"""
    	# 用户POST提交数据，数据校验
    	emp_add_form = EmpAddForm(data = request.POST)
    	if emp_add_form.is_valid():
    		#POST数据合法，cleaned_data就是整理好的数据
    		print(emp_add_form.cleaned_data)
            # cleande_data：一个字典，键值对存储数据
    		#Employee.objects.create(...)
    		emp_add_form.save()
    		return redirect("/emp_list/")
    	else:
    		#POST数据不合法，errors就是报错信息
    		print(emp_add_form.errors)
    		return render(request,"emp_add.html",{"emp_add_form":emp_add_form})
    	emp_add_form = EmpAddForm()
    	return render(request,"emp_add.html",{"emp_add_form":emp_add_form})
    ```

    ![image-20240511161447610](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111614045.png)

    还有其他类型报错信息，比如我们给那么加上一个约束（在ModelForm中也能加）

    ![image-20240511161727994](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111617380.png)

    ![image-20240511162013360](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111620281.png)

    报错信息为英文，可在 settings.py中 配置LANGUAGE_CODE = "zh-hans"

###### 6.2.4 员工删除

```python
# urls.py
path('emp_del/', views.emp_del),

# views.py
def emp_del(request):
    """员工删除"""
    emp_del_id = request.GET.get("emp_del_id")
    Employee.objects.filter(id=emp_del_id).delete()
    return redirect("/emp_list/")

```

```python
<a class="btn btn-danger btn-xs" href="/emp_del/?emp_del_id={{ emp.id }}">删除</a>
```

###### 6.2.5 员工编辑

步骤分析：

- 点击编辑，跳转页面
- 编辑页面（根据id获取数据，并将默认值显示在前端）
- s获取前端数据，校验
- 更新数据库

```python
{# emp_list.html #}
<a class="btn btn-primary btn-xs" href="/emp/{{ emp.id }}/update/">编辑</a>

{# urls.py #}
path('emp/<int:nid>/update/', views.emp_update),

```

```python
class EmpUpdateForm(forms.ModelForm):
	"""员工编辑的ModelForm"""
	class Meta:
		model = Employee
		fields = ['name','password','age','account','create_time','gender','dep']
	
	def __init__(self,*args,**kwargs):
		super().__init__(*args,**kwargs):
		# 循环找到所有的插件，然后添加样式
		for name,field in self.fields.items():
			# 如果某些输入框的样式不想调整，可以增加判断
			if name == "password":
				continue
			field.widget.attrs = {"class":"form-control","placeholder0:field.label"}
	def emp_update(request,nid):
	"""基于ModelForm实现的员工新建"""
	# 用户POST提交数据，数据校验
    default_emp = Employee.objects.filter(id=nid).first()
	emp_update_form = EmpUpdateForm(data = request.POST,instance=default_emp)
	if emp_update_form.is_valid():
		#POST数据合法，cleaned_data就是整理好的数据
		print(emp_update_form.cleaned_data)
        # cleande_data：一个字典，键值对存储数据
		#Employee.objects.create(...)
		emp_update_form.save()
		return redirect("/emp_list/")
	else:
		#POST数据不合法，errors就是报错信息
		print(emp_update_form.errors)
		return render(request,"emp_update.html",{"emp_update_form":emp_update_form})
	emp_update_form = EmpAddForm(instance = default_emp)
	return render(request,"emp_update.html",{"emp_update_form":emp_update_form})
```

**注意**：

- ModelForm也提供了相关方法来显示默认值，首先根据id获取数据库中相应的那行值（QuerySet类型数据），然后通过ModelForm提供的instance参数传入数据，ModelForm就会默认把传入的那个对象展示到前端了
- save( )方法会创建而不是修改，若想修改，那么在post请求中创建的emp_update_form也需要传入instance参数，让EmpUpdateForm对象知道改哪一条记录的值

**代码优化：**

- emp_update( )和emp_add( )可以共用一个EmpForm
- save( )方法会把前端用户输入的数据存入数据库中，但用户输入的数据和表中字段得对应才行，假设传入两个数据，数据库一行得要三个参数，剩一个参数是后台给一个默认值和前端的两个数据一起写入数据库。
    - 解决方法：可以引用 instance参数，手动赋值进去即可（不过这个字段名是必须得在数据表中存在的哦！）
- 创建时间的格式问题：原因是数据库表设计的时候不合理，把`create_time`的`DateTimeField()`改成`DateField()`，然后两句指令跑一下，刷新页面即可。

##### 6.3 条件搜索

###### 6.3.1 所用知识点：filter

filter( )函数用法：

```python
search_dict = {
    "name": "zhuming",
    "dep": "IT部",
}
models.Employee.objects.filter(**search_dict)

# 等同于
models.Employee.objects.filter("name": "zhuming", "dep": "IT部")

```

大于小于的用法

```python
# 数字的一些符号
id=12	# id=12的筛选出来
id__gt=12	# id>12的筛选出来
id__gte=12	# id>=12的筛选出来
id__lt=12	# id<12的筛选出来
id__lte=12	# id<=12的筛选出来

search_dict = {
    "id__lte": 12,
}
models.Employee.objects.filter(**search_dict)

```

```python
# 字符串的一些符号
phone = "17732101234"	# phone = "17732101234"的筛选出来
phone__startswith="177"	# "177"开头的筛选出来
phone__endswith="1234"	# "1234"结尾的筛选出来
phone__contains="321"	# 包含"321"的筛选出来

search_dict = {
    "phone__contains": "12",
}
models.Employee.objects.filter(**search_dict)

```

Q用法

```python
# 使用 Q对象查询：
# 在 Django ORM 中，使用 Q 对象来构建复杂的 OR 查询条件。
from django.db.models import Q

# 查询数据库
orders = Orders.objects.filter(**query_params).filter(
    Q(entrust_status=1) | Q(entrust_status=2)).order_by('-entrust_time').all()
```

###### 6.3.2 实现功能

```python
def emp_list(request):
	"""员工管理"""
	search_filter = {}
	search_data = request.GET.get("search")
	if search_data:
		search_filter["name__contains"] = search_data
	emp_data = Employee.objects.filter(**search_filter).order_by("id")
	# 调整数据格式
	for emp in emp_Data:
		emp.create_time = emp.create_time.strftime("%Y-%m-%d")
		emp.gender = emp.get_gender_display()
		emp.dep_id = emp.dep.dep_name
	return render(request,"emp_list.heml",{"emp_queryset":emp_data})
```

注意：有个缺点，输入框点击搜索后就清空了，用户很容易忘了自己是搜什么的，所以，后端传递参数时，多传一个；前端再设置input框的value值。

```python
return render(request, "emp_list.html", {"emp_queryset": emp_data, "search_data": search_data})
```

```python
<input type="text" id="search" name="search" class="form-control" value="{{ search_data }}" placeholder="请输入员工姓名 支持模糊查询">
```

一开始输入框显示None了，所以我们要给他赋值一个空字符串，不能不管（None就是因为不管才出现的）

```python
if search_data:
		search_filter["name__contains"] = search_data
else:
	search_data = ""
```

##### 6.4 分页显示

###### 6.4.1所用知识点

和切片很像，就是不会越界（如果QuerySet的列表不够10个数据，它不会报错，而是有几个就输出几个）。

```python
# filter和all都可以
Employee.objects.all()	# 获取全部数据
Employee.objects.all()[0:10]	# 获取前10条数据

```

###### 6.4.2 实现功能(尚未实现)

##### 6.5时间选择组件

这里我们调用的是bootstrap-datepicker插件，调用的语法如下：

```html
<link rel="stylesheet" href="{% static 'plugins/bootstrap-3.4.1-dist/css/bootstrap.css' %}">
<!-- datepicker必须提前引入bootstrap的css -->
<link rel="stylesheet" href="{% static 'plugins/datepicker.css' %}">
<!-- 后面会利用id来写js -->
<input id="dt" type="text" class="form-control" placehorder="入职时间">

<script rel="script" src="{% static 'js/jquery-3.6.0.min.js' %}"></script>
<script rel="script" src="{% static 'plugins/bootstrap-3.4.1-dist/js/bootstrap.js' %}"></script>
<!-- datepicker必须提前引入JQuery和bootstrap的js -->
<script rel="script" src="{% static 'plugins/bootstrap-datepicker.js' %}"></script>
<!-- 调用datepicker设置显示格式 -->
<script>
    $(function () {
        $('#dt').datepicker({
            format: 'yyyy-mm-dd',
            startDate: '0',
            language: 'zh-CN',
            autoclose: true,
        });
    })
</script>

```

我们用这个插件来完善一下我们的emp_add.html，因为我们是用ModelForm来生成的页面，所以我们怎么通过调用input框的id来实现插件呢？

其实，我们右击网页点检查，会发现，ModelForm在帮我们渲染前端输入框时，默认都会有一个id（命名格式：id_+字段名），所以我们直接用即可。

##### 6.6ModelForm样式优化

前面我们说ModelForm说过，ModelForm渲染在前端的输入框没有任何样式，需要我们通过widget来设置其属性及属性值。

![image-20240511170542871](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111705538.png)

然后我们又介绍了快速给所有输入框上样式的方法，通过`__init__()`去实现。

![image-20240511170554935](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111705420.png)

但是，现在我们再看这个方法，有些不严谨。如果输入框原本有一些样式的话，通过这段代码，不管你原本有无样式，`field.widget.attrs`都会给你定死成`"class": "form-control", "placeholder": field.label`。

所以我们要来完善一下逻辑。

![image-20240511170617873](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111706245.png)

But，如果以后我们用到ModelForm的话，我们这段代码得重复写。所以我们还可以将这个`__init__()`方法写到utils中去。

![image-20240511170628951](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111706343.png)

然后我们在`views.py`中需要写ModelForm的时候，就不去继承`forms.ModelForm`了，而是去继承`em_web.utils.bootstrap`中的BootstrapModelForm了。

![image-20240511170640573](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111706367.png)

这样，我们以后就不需要每次都继承`__init__()`方法去上样式了。



**注意：**

- 添加样式：添加的类名，好像是通过bootstrap根据类名加的样式，所以我拿到一个新的html文件做修改时，应该根据其类名来设置类名
