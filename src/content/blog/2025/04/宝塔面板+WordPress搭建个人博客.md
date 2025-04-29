---
title: "宝塔面板+WordPress搭建个人博客"
categories: code
tags: ['宝塔面板', 'WordPress', '个人博客']
id: "864705ce74b971d2"
date: 2025-04-29 13:23:01
cover: "https://wp-cdn.4ce.cn/v2/AkNoKSH.jpeg"
---

:::note
宝塔面板+WordPress搭建个人博客
:::

## 宝塔面板+WordPress搭建个人博客

1. 下载宝塔：[宝塔面板下载，免费全能的服务器运维软件 (bt.cn)](https://www.bt.cn/new/download.html)

    ```
    ==================================================================
    Congratulations! Installed successfully!
    =============注意：首次打开面板浏览器将提示不安全=================
    
     请选择以下其中一种方式解决不安全提醒
     1、下载证书，地址：https://dg2.bt.cn/ssl/baota_root.pfx，双击安装,密码【www.bt.cn】
     2、点击【高级】-【继续访问】或【接受风险并继续】访问
     教程：https://www.bt.cn/bbs/thread-117246-1-1.html
     mac用户请下载使用此证书：https://dg2.bt.cn/ssl/mac.crt
    
    ========================面板账户登录信息==========================
    
     【云服务器】请在安全组放行 10326 端口
     外网面板地址: https://47.93.10.129:10326/4541015a
     内网面板地址: https://172.25.140.136:10326/4541015a
     username: 92bmyejf
     password: d13afcca
    
     浏览器访问以下链接，添加宝塔客服
     https://www.bt.cn/new/wechat_customer
    ==================================================================
    
    ```

    

2. 安全组开放对应端口

3. 下载LNMP环境

4. 创建网站站点

    - 关闭服务器下载的nginx，重新启动宝塔下载的nginx版本，修改配置文件
        - root 后跟文件路径
        - index 后跟html文件
        - listen 后跟监听端口

5. WordPress搭建个人博客

    - ```
          
        数据库账号资料
        
        数据库名：sql39_101_69_19
        
        用户：sql39_101_69_19
        
        密码：cr7JtrTTz8
        
        访问站点：http://39.101.69.19/index.php
        ```

6. 访问站点登录即可

