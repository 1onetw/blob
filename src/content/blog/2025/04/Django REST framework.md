---
title: "Django REST framework"
categories: code
tags: ['Django', 'RESTful API']
id: "62996eed07abb06c"
date: 2025-04-29 15:48:30
cover: "https://wp-cdn.4ce.cn/v2/3jWUEyJ.png"
---

:::note
Django REST framework
:::

# Django REST framework

## 1. 前后端分离

### 1.1  理解

1. 交互形式

    - 基于协议：HTTP、HTTPS
    - 基于接口规范：RESTful API
    - 基于数据格式：JSON/XML

2. 组织方式

    - 前后端代码分离
    - 前后端独立部署

3. 开发模式

    - 混合开发
        - 提出需求 --> 前端页面开发 --> 翻译成模板 --> 前后端对接 --> 集成遇到问题 --> 前端返工 --> 后端返工 --> 二次集成 --> 集成成功 --> 交付上线
    - 前后端分离开发
        - 提出需求 --> 约定接口规范、数据格式 --> 前后端并行开发 --> 前后端对接 --> 前端调试效果 --> 集成成功 --> 交付上线	

4. 数据接口规范流程

    ![Snipaste_2024-05-11_19-46-21](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202405111948899.png)

### 1.2 优势

- 多终端性
- 提升开发效率：代码解耦，前后端并行开发
- 提高代码可维护性：前后端独立部署

## 2. RESTful API

### 2.1 RESTful API接口规范

### 2.2 HTTP请求方法

1. GET（SELECT）：从服务器取出资源（一项或多项）
2. POST（CREATE）：在服务器新建一个资源
3. PUT（UPDATE）：在服务器更新资源（客户端提供完整资源）
4. PATCH（UPDATE）：在服务器更新资源（客户端提供部分资源）
5. DELETE（DELETE）：从服务器删除资源
6. HEAD：获取资源的元数据
7. OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的

## 3. 搭建django项目

1. 创建项目
2. 创建app
3. 注册app
4. 配置数据库
5. 配置后台语言
6. 配置时区
7. 配置静态文件
8. 创建后台超级用户
9. 启动项目