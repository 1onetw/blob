---
title: "Django母婴商城项目从0到1"
categories: project
tags: ['Django', '网站开发']
id: "2391d02d0bb68706"
date: 2025-04-29 15:25:49
cover: "https://wp-cdn.4ce.cn/v2/Smn89fx.jpeg"
---

:::note
Django母婴商城项目从0到1
:::

# 母婴商城

## 主页

### 后端数据库查询

![image-20240513160110219](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131601470.png)

```
locals()：将定义的所有变量传到模板文件index.html

列表生成式：查出一级分类下的二级分类

types__in = cl：此类型包含于cl列表
```

### 前端

#### 静态文件和媒体文件

```
静态文件：{% load static %} --> {% static '' %}
媒体文件：{{ clothe.img.url }}：
	clothe：数据库的一条记录，循环出来的对象
	img：字段，值为url，media下的地址，不加media
	url：django语法，会将其渲染为访问媒体资源的url
```

#### 模板文件

![image-20240513162210015](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131622352.png)

![image-20240513162258991](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131623422.png)

![image-20240513163012047](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131630155.png)

```
pic1：循环展示数据，根据forloop.counter(循环次数)决定展示前4条数据
pic2：循环展示数据，根据forloop.counter(循环次数)决定展示后4条数据
pic3：layui一个前端框架(已不更新)，实现轮播图的使用方法
```

#### 导航栏是否选中样式

![image-20240513164138528](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131641505.png)

```
根据后台传入的classContent的值来决定是否有class="active"属性
```

## 商品页

### 后端数据库查询

![image-20240513170424454](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131704649.png)

![image-20240513170043957](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131700131.png)

![image-20240513170242580](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131702717.png)

![image-20240513170257689](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131702729.png)

```
# 根据模型Types生成商品分类列表：用于前端循环渲染
	Types.objects.values("firsts").distinct()：根据firsts去重，选每个firsts的第一个作为QuerySet的元素
# 获取请求参数：
	t：商品分类
	s：排序类型
	p：当前页数
	n：模糊查询
# 根据请求参数查询商品信息
# 分页功能：
	固定语法，django自带功能，需要导入最上面的3个包
	含义：
		将查到的所有商品信息(QuerySet)进行分页，每页6条数据
		try：第p页的数据
		except PageNotAnInteger：显示第一页的数据
		except EmptyPage：显示最后一页数据
```

### 前端

#### js实现相关页面样式

![image-20240513175809401](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131758553.png)

```
休整一下，js加引号
```

![image-20240513171501401](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131715390.png)

![image-20240513171519193](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131715166.png)

```
layui一个前端框架，使用得引入layui那个文件夹，好像还用到了一个mm.js文件
如：
	生成分页条代码：在id=demo0的div里生成，生成完，f12复制粘贴到id=demo0的div里，再注释即可
	一级二级分类样式
```

#### 导航分类

![image-20240513173213076](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131732927.png)

![image-20240513182517928](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131825907.png)

```
将超链接地址进行修正
```

#### 商品列表

![image-20240513173810538](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131738633.png)

```
commodityInfos|length：过滤器，这个QuerySet的长度

p.price|floatformat:'2'：过滤器，保留两位小数

{% url 'commodity:detail' p.id %}：链接到主路由commodity下的子路由detail，并传参p.id

pages.object_list：page对象的全部数据
```

#### 分页

![image-20240513181740629](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131817284.png)

![image-20240513191801148](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131918167.png)

#### 排序

![image-20240513183451275](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131834018.png)

![image-20240513183522123](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131835313.png)

```
修改超链接地址，地址的请求参数不同获得的商品数据不同

根据class修改选中样式
```

#### 搜索

![image-20240513184252479](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405131842553.png)

```
1. 修改超链接地址
2. input标签 name="n"
3. button带一个submit属性，让其将input标签相关数据提交到action属性中
```

## shopper子路由

![image-20240520164541549](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201645703.png)

```
分别实现：
	登录注册逻辑
	个人中心
	购物车
	退出
	支付
```



#### 登录注册逻辑

##### cookie和session

```
cookie存放sessionID和不私密信息，存放于用户端
与sessionID对应的session存私密信息，存放于服务器端

注册：输入用户名、密码、手机号、验证码，生成session存放于服务器端
登录：输入用户名、密码，若用户名、密码正确，生成cookie，其中sessionID与之前注册生成的session相对应
```

##### 登录注册逻辑需引入的包

![image-20240520163733415](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201637120.png)

```
支付相关方法需要引入相关包，否则报错

支付相关方法上面的包是django自带的
```



##### 后端loginView

![image-20240520163701829](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201637793.png)

![image-20240520175941256](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201759908.png)

```
LoginModelForm(data=request.POSST)：传入data属性
form.py中利用data属性编写 数据清洗的方法

authenticate函数是Django中用于验证用户身份的函数之一。它的作用是根据提供的用户名和密码，在认证后端中查找相应的用户，并验证密码是否匹配。

具体来说，authenticate函数会遍历所有已配置的认证后端（例如数据库后端、LDAP后端等），尝试使用提供的用户名和密码进行认证。如果找到了匹配的用户，并且该用户的密码也与提供的密码匹配，则authenticate函数会返回对应的用户对象；否则，返回None。

authenticate函数的常见用法是在用户登录时使用，通过验证用户名和密码来判断用户身份是否合法。如果验证成功，则可以调用login函数将用户标记为已登录状态。

authenticate函数向数据库中的用户表去查找用户。在Django中，默认情况下，用户表名为auth_user，对应的模型是django.contrib.auth.models.User。这个表存储了用户的基本信息，包括用户名、密码（经过哈希处理）、邮箱等。

当你调用authenticate函数时，它会在auth_user表中查找与提供的用户名匹配的用户记录，并验证密码是否正确。这是Django默认的认证后端行为，你也可以通过配置来修改默认的认证后端，使用自定义的用户模型或认证后端。

login方法，将user的id写入服务器端session中，后续可通过request.user.id获取用户id，如获取相关信息进行支付功能，花的就是这个用户的钱 	
```

##### form.py用于实现登录页面 输入框

![image-20240520163112139](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201637318.png)

##### 前端页面使用

```
自动生成对应的input标签
```

![image-20240520163503044](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201635115.png)

```
前端表单提交：
	用的是一个layui开源前端框架的表单，用的是他的提交方法
	使用时查看其官方文档
```

![image-20240520172020453](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201720273.png)

![image-20240520172235160](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201722229.png)

![image-20240520172322861](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201723580.png)

```
解决跨域请求问题，
增加csrf参数
```

![image-20240520172807698](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201728828.png)

```
添加一个p标签，用于显示提示信息
并在ajax请求的回调函数里将提示信息放到p标签中
```

![image-20240520175843049](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201758104.png)

### 退出

```
清楚session并退出，如果没有退出，一直待机，那么session也应该有一个默认的有效期，到期后就需要重新登录。
还可以登录时设置7天免登录，将session的有效期设置为7天
reverse将'shopper:login'转换为url格式
redirect页面重定向(页面跳转)
```

![image-20240520182438376](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201836804.png)

![image-20240520181204472](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201812605.png)

### 个人中心

#### 模型层

购物车表

![image-20240520180643544](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201806929.png)

订单表

![image-20240520180658031](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201807660.png)

```
Meta相关信息：用于后台设置，如后台显示表名为verbose_name的值
choices=STATE：一个关系映射，数据库中存储更少的数据来映射到相应的数据
```

#### 控制层

ShopperView：个人中心

![image-20240520182438376](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201824247.png)

![image-20240520182130878](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201821089.png)

![image-20240520182215557](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201822094.png)

#### 视图层

```
自己团一下就好

目前前端框架：layui 和 vue
	本质一样，都有现成的组件(html+css)，也提供了自己写js该怎么写
```

### 购物车

#### 模型层

购物车表

![image-20240520180643544](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201905622.png)



#### 控制层

![image-20240520190456688](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201904556.png)

![image-20240520190558384](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201905117.png)

#### 视图层



```
通过商品id 对字典进行查询 获取到商品对应的数量
也就是说：根据一个变量查询到字典对应key的值
要用自定义过滤器(get_item)
```

![image-20240520190953807](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201909549.png)

```
自定义模板过滤器，模板文件可以使用其对数据进行处理
```

![image-20240520191151024](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201911934.png)

```
js实现：
	单个商品总价格 随改商品数量加减进行求和，
	总价格也随之变化
```

![image-20240520191448458](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201914334.png)

![image-20240520191620692](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201916431.png)

![image-20240520191650609](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201916305.png)

### 支付

#### 控制层

![image-20240520191929903](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201919648.png)

```
the_time：当前时间
payInfo：订单信息
结算的话 就将当前时间和订单信息存入session中用于生成订单
return_url：支付成功后跳转的页面
url就是支付成功后跳转的页面，也就是return_url
```

![image-20240520192938767](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201929505.png)

```
个人中心中，根据支付成功传过来的参数进行订单的生成
```

### get_pay方法

1. 安装对应的aliyun支付相关包：pip安装会失败，因为其中一个依赖包(pycrypto:加密算法)不兼容win10，所以采取以下步骤进行安装

2. ![image-20240520193957734](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201939661.png)

    下载.gz包，进行解压

3. ![image-20240520194119802](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201941664.png)

    解压之后，将setup.py中第47行中的pycrypto改为crypto

4. ![image-20240520194242303](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405201942169.png)

    输入python .\setup.py install进行安装

## django自带后台系统

