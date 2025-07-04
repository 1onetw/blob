---
title: "VSCode使用git常见问题"
categories: code
tags: [ 'VSCode使用Git常见问题' ]
id: "351660b5aa28d2f3"
date: 2025-05-11 21:13:07
cover: "https://wp-cdn.4ce.cn/v2/7K35VGW.jpeg"
---

:::note
VSCode使用Git常见问题
:::

# VSCode使用Git

现状：

1. 使用ssh连接远程仓库
2. 使用key连接尚存在问题

常见问题：

1. 连接不到远程仓库，内容push不上去

    ```
    重新设置远程仓库即可：
    1.删除目前绑定的远程仓库
    git remote ：简单查看绑定的远程仓库
    git remote -v ：详细查看绑定的远程仓库
    git remote remove origin ： 删除绑定的远程仓库origin
    
    2. 重新绑定远程仓库
    git remote add origin [SSH地址]
    ```

2. 当前分支落后于远程分支：

    ```
    拉取到本地进行合并即可：
    git pull origin main
    ```

3. branch分支领先于main分支：

    ```
    1. VsCode合并分支
    2. GitHub进行pull request，请求合并分支，若项目绑定云部署，则会产生branch分支的测试环境
    ```

4. 当前分支领先于远程分支：

    ```
    推送并合并即可：
    git push -u origin main
    
    注：-u是--set-upstream的缩写，其核心作用是建立本地分支与远程分支的关联关系，使得后续操作（如git push或git pull）无需再指定远程仓库和分支名称
    ```

5. 基于本地文件夹创建GitHub仓库

    ```
    1.初始化仓库
    2.提交到本地仓库
    3.发布分支到远程仓库（注意不能有过大文件：大于100MB，会推送失败）
    ```

6. 若提交到本地仓库的内容推送不上去：因为存在敏感信息，需要删除敏感信息

    ```
    回退提交删除敏感信息重新提交、推送即可
    # 1. 回退到上一个提交（保留本地修改）
    git reset HEAD~1

7. ssh: connect to host github.com port 22: Connection refused

   ```markdown
   将连接端口22改为443，在.ssh目录下 创建一个config文件，并在文件里添加以下内容：
   Host github.com
      Hostname ssh.github.com
      Port 443
