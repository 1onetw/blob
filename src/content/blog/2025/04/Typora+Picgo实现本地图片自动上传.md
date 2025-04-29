---
title: "Typora + Picgo实现本地图片自动上传"
categories: code
tags: ['Typora', 'Picgo', '图床配置']
id: "0ebf280688819a13"
date: 2025-04-29 13:39:01
cover: "https://wp-cdn.4ce.cn/v2/QyiQpV3.jpeg"
---

:::note
Typora + Picgo实现本地图片自动上传
:::

# Typora+PicGo+Github

目的：笔记中存放图片，更加方便

1. 下载PicGo、Typora

2. 注册Github

3. 打开Github需要加速器（UU加速器、Steam++ ）

    - UU加速器进行 "学术资源" 加速
    - Steam++ 进行加速

4. PicGo配置好Github的仓库

    token需要自己去生成：参考（[使用Github+picGo搭建图床，超详细教程_github图床挂了-CSDN博客](https://blog.csdn.net/m0_64037602/article/details/130189259)）

    ![微信截图_20231129214220](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202311292143969.png)

5. 记录下PicGo配置Github的token（需要自己去github生成）

    ```
    [github token]
    
    2025年12月1日到期，到时候需要更新token，否则将失去对github仓库的访问权限
    ```

6. Typora设置图片应用PicGo自动上传并测试

    ![微信截图_20231129214513](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202311292145456.png)

**备注：**使用时因为图片要上传至gitbub。所以最好使用时打开UU加速器进行学术加速，否则可能上传失败

![Snipaste_2024-12-09_10-32-01](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202412091032444.png)