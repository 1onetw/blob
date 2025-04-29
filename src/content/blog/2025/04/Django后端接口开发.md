---
title: "Django后端接口开发"
categories: code
tags: ['Django', '后端接口开发']
id: "6abe52459b3e515d"
date: 2025-04-29 15:35:02
cover: "https://wp-cdn.4ce.cn/v2/U2vKNBq.jpeg"
---

:::note
Django后端接口开发
:::

# Django后端接口开发

## 1. QuerySet

### 1.1 QuerySet描述

数据库查询的结果为QuerySet

### 1.2 QuerySet操作

1. filter( )
2. all( )
3. delete( )
4. update( )

## 2. Instance

### 2.1 介绍

```
Instance指的是一个Django模型的单个实例，也就是数据库中的一行数据。

相比于QuerySet(查询集合)，它是针对单个对象的操作，用于创建、更新或删除单个模型实例
```

### 2.2 操作

- 创建一个对象：Obj = Model(attr1=val1, attr2=val2)，Obj.save()
- 更新一个对象：Obj = Model.objects.get(id=xxx)，Obj.attr1 = val1，Obj.save()
- 删除一个对象：Obj = Model.objects.get(id=xxx)，Obj.delete()

## 3. APIview

```
View是Django默认的视图基类，APIView是REST framework提供的所有视图的基类, 继承自Django的View。

APIview 可以处理基于 HTTP 协议的请求，并返回基于内容协商的响应，它旨在提供一个易于使用且灵活的方式来构建 API 视图。
```

APIview应用得以实现，需要有以下三个部分：

1. models.py里创建的数据表
2. view.py里对 QuerySet数据 查询处理功能 的程序代码
3. 并通过urls.py实现和网页的链接。

## 4. serializers及应用

序列化器，准确说是数据类型转化器

#### 4.1 功能意义

```
将 Django数据库中的数据（queryset 或者instance ）转换为 json/xml/yaml数据格式，返回给前端过程称为序列化，相反过程称为反序列化。而实现这一过程运用的工具称为称为序列化器。Django提供的强大序列化工具叫做serializers（序列化器）。
```

#### 4.2 执行过程

1. 获取对象：
    - data = Goods.objects.get(id=1)
    - data = Goods.objects.all() 
2. 创建序列化器：
    - serializer = GoodsSerializer(instance=data) 
    - serializer = GoodsSerializer(instance=data,many=True)
3. 输出数据：
    - print(serializer.data)

#### 4.3 应用举例-序列化转化的可视化表示

##### 4.3.1 编程思路

1. 创建项目，创建app，注册app，配置数据库p
2. 编写models.py，提供序列化的数据源
3. 编写serializer.py，定义序列化器
4. 编写view.py，实现序列化及其可视化功能
5. 编写urls.py，构建url和view.py功能函数之间链接

##### 4.3.2 编程实现

###### models.py

```python
    # 导包
    
    from django.db.models import *
    
   # 创建数据表
   
       class GoodsCategory(Model):       #产品分类表
		     name = CharField(max_length=64, verbose_name='名称')
			remark = CharField(max_length=256, null=True, blank=True, verbose_name='备注')
			
       class Goods(Model):      #产品明细表
   		number = CharField(max_length=64,null=True, blank=True,verbose_name='编号')     				    
   		name=CharField(max_length=64,null=True,blank=True,default=0,verbose_name='名称')
       	barcode = CharField(max_length=32,null=True, blank=True, default=None,verbose_name='条码')
      	category = ForeignKey(GoodsCategory, on_delete=SET_NULL,  null=True,blank=True,
      	default=None,related_name='goods_set', verbose_name='产品分类')
         spec = CharField(max_length=64, null=True, blank=True, verbose_name='规格')
     	shelf_life_days = IntegerField(null=True, blank=True, verbose_name='保质期天数')
    	purchase_price = FloatField(default=0,null=True,blank=True,verbose_name='采购价')
    	retail_price = FloatField(default=0,null=True,blank=True,verbose_name='零售价')
    	remark = CharField(max_length=256, null=True, blank=True, verbose_name='备注')

```

###### serializer.py

```python
       # 导包
       
       from rest_framework.serializers import *
       from .models import *
     
      #定义产品分类序列化器
      
      class GoodsCategorySerializer(ModelSerializer):# 产品分类序列化器
             class Meta:
                  model = GoodsCategory
                  fields = ('name', 'remark')

     class GoodsSerializer(ModelSerializer):# 产品序列化器
         category = GoodsCategorySerilizer() #外键字段相关的数据 需要单独序列化
			class Meta:
				model = Goods
                fields = ('name',)# 序列化单个字段
                fields = ('name','number',)# 序列化多个字段
                fields = ('name','number',' category ')# 序列化多个字段
                fields = '__all__'# 序列化所有字段
                (上述fileds任意选一，或者编程实现多选一)

```

###### views.py

```python
##导包

	from django.shortcuts import render
	from rest_framework.response import Response
	from .models import *
	from rest_framework.decorators import api_view
	from django.shortcuts import get_object_or_404
	from rest_framework.views import APIView
	from .serializer import *

 ##数据获取、序列化转化及其可视化
 
 class GetGoods(APIView):
        def get(self, request):
               data = Goods.objects.all()
               serializer = GoodsSerializer(instance=data, many=True)
              # serializer = GoodsSerializer(instance=data, name=电影) 
				print(serializer.data)
				return Response(serializer.data)
 		def post(self, request):     # 从请求数据中提取字段
 				request_data = {
					"category": request.data.get("Goodscategory"),
					"number": request.data.get("number"),
					"name": request.data.get("name"),
					"barcode": request.data.get("barcode"),
					"spec": request.data.get("spec"),
					"shelf_life_days": request.data.get("shelf_life_days"),
					"purchase_price": request.data.get("purchase_price"),
					"retail_price": request.data.get("retail_price"),
					"remark": request.data.get("remark"),
				}
		# 使用 create() 方法创建新的商品对象
					new_goods = Goods.objects.create(**request_data)
		# 对创建的对象进行序列化，并作为响应返回
				serializer = GoodsSerializer(instance=new_goods)
							return Response(serializer.data)

```

###### urls.py

```python
# 导包

	from django.contrib import admin
	from django.urls import path
	from apps.erp_test.views import *

# url与view.py里函数映射表

	urlpatterns = [
		path('admin/', admin.site.urls),
		path('getgoods/', GetGoods.as_view()),
	]

```

##### 4.3.3 输出结果

