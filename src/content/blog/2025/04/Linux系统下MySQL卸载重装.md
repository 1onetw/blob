---
title: "Linux系统下MySQL卸载重装"
categories: question
tags: ['mysql卸载重装']
id: "266c7af8da6c216e"
date: 2025-04-27 16:59:53
cover: "https://wp-cdn.4ce.cn/v2/1Nhr1Mo.jpeg"
---

:::note
Linux系统下MySQL卸载重装
:::

# Linux系统下mysql卸载重装

## 完全卸载

一、彻底卸载：

1.停用服务： systemctl stop mysqld

2.把mysql从系统中卸载：（中途要输入y确定卸载）

yum remove mysql-common-8.0.26-1.module_el8.4.0+915+de215114.x86_64

3.查看mysql的情况 卸载干净否：rpm -qa|grep -i mysql

无输出就是卸载干净了，有输出下一步 如输出：MySQL-client-5.5.25a-1.rhel5，

4.输出啥直接复制下来 前面加rpm -ev 给删掉：

如rpm -ev MySQL-client-5.5.25a-1.rhel5

5.查看mysql目录：find / -name mysql

输出啥一个个复制路径下来前面加rm -rf 路径 直接全删掉

查找目录如下：

/var/lib/mysql /var/lib/mysql/mysql

6.删除对应的mysql目录：

rm -rf /var/lib/mysql rm -rf /var/lib/mysql

7.删掉my.cnf:

rm -rf /etc/my.cnf

8.输入：rpm -qa|grep -i mysql

不显示任何结果说明卸载干净了 显示啥rm -rf删掉 直到没有任何输出就卸载干净了

## 安装

宝塔面板进行安装mysql服务
