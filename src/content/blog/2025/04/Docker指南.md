---
title: "Docker指南"
categories: code
tags: ['Docker指南', 'Docker部署']
id: "62f6ba41a141ca05"
date: 2025-04-29 15:54:32
cover: "https://wp-cdn.4ce.cn/v2/8OocSFA.jpeg"
---

:::note
Docker指南
:::



# Docker背景

## 什么是docker

```
Docker,开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可抑制的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。
完全使用沙盒机制，不依赖于任何语言、框架或者包装系统。
```

**小知识**：**沙盒**也叫沙箱（sandbox）。在计算机领域指一种虚拟技术，而且多用于计算机安全技术。安全软件可以让它在沙盒中运行，如果含有恶意行为，则禁止程序的进一步运行，而这不会对系统造成任何危害。

```
Docker是dotCloud公司开源的一个基于LXC的高级容器引擎，源码托管在Github上，基于**go语言**并且遵从Apache2.0协议开源。
```

**GitHub地址**：https://github.com/moby/moby

**小知识**：LXC为Linux Container的简写。Linux Container 容器是一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源，而且不需要提供指令解释机制以及全虚拟化的其他复杂性。
LXC主要通过Kernel的namespace实现每个用户实例之间的项目隔离，通过cgroup实现对资源的配额和调度。

docker官网：https://www.docker.com
docker中文库:https://www.docker.org.cn/

## Docker容器技术和虚拟机的区别

**相同点：**都是虚拟化技术

**不同点：**

docker不需要Hypervisor实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。所以docker效率比虚拟机效率高。达到了秒级启动的地步。

**docker相较于VM的优点**：
1、比VM小、快，Docker容器的尺寸减小相比于整个虚拟机大大简化了分布
到云和分发时间的开销。Docker启动一个容器实例时间仅仅需要几秒钟。

2、Docker是一个开放的平台，构建、发布和运行分布式应用程序。

3、开发人员不需要关系具体是哪个Linux操作系统

4、Google、微软（azure）、亚马逊、IBM等都支持docker。

5、Docker支持Unix/Linux操作系统，也支持Windows和Mac。

**Docker局限性**：

只用于计算，存储需要通过外部挂载等方式将日志、数据库等放在Docker容器外。

## 通过docker架构图初步了解docker

![image-20241109082021227](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090820475.png)

**工作流程：**
1、启动docker
2、下载镜像到本地
3、启动docker容器实例
提示：大家可以去注册一个dockerhub，之后会详细给大家讲解它的作用（非常重要！连docker hub账号都没有，玩什么docker！）。

**Docker核心技术**:
1、Namespace —> 实现Container的进程、网络、消息、文件系统和主机名的隔离。
2、Cgroup —> 实现对资源的配额和调度。
注意：Cgroup的配额，可以指定实例使用的CPU个数，内存大小等。

## Docker特性

- 文件系统隔离：每个进程容器运行在一个完全独立的根文件系统里
- 资源隔离：系统资源，像CPU和内存等可以分配到不同的容器中，使用cgroup
- 网络隔离：每个进程容器运行在自己的网络空间、虚拟接口和IP地址
- 日志记录：Docker将收集到和记录的每个进程容器的标准流（stdout/stderr/stdin），用于实时检索或者批量检索
- 变更管理：容器文件系统的变更可以提交到新的镜像中，并可重复使用以创建更多的容器。无需使用模板或者手动配置
- 交互式shell：Docker可以分配一个虚拟终端并且关联到任何容器的标准输出上，例如运行一个一次性交互shell

# Docker的安装

## 部署docker容器虚拟化平台并且配置docker镜像加速地址

实验环境：centos7。网络要求能上外网

## 安装docker依赖环境

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 配置国内docker-ce的yum源

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

命令查看有没有配置成功：

```
cd /etc/yum.repos.d
ls
```

![image-20241109083425547](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090834671.png)

## 安装docker

```
yum -y install docker-ce doker-ce-cli containerd.io
```

## 开启网络转发功能

默认会自动开启。
路径 ：**/proc/sys/net/ipv4/ip_forward**
手动开启：

```
vim /etc/sysctl.conf   #插入以下内容
net.ipv4.forward =1
-------------------------
sysctl -p   #生效
cat /proc/sys/net/ipv4/ip_forward  #查看结果，为1开启成功。

```

**如果没有开启网络转发，我们启动实例的时候就会报错！！！**

关闭防火墙：

```
iptables -nL #查看一下iptable规则，关闭防火墙后会自动插入新规则

systemctl stop firewalld && systemctl disable firewalld  #关闭防火墙

sysctlrem restart docker # 关闭防火墙要把docker重启一下，不然docker
的ip包转发功能无法使用。即便防火墙关闭了，docker依旧会调用内核模块netfilter增加规则，所以会新增iptables规则

iptables -nL #再查看一下iptable规则，会发现多出很多规则

```

**iptables -nL**

![image-20241109083906091](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090839275.png)

## 启动服务

```
systemctl start docker && systemctl enable docker
```

启动完成后会改网络参数，这个是ip转发会改成1。默认0

![image-20241109084028497](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090840556.png)

```
docker version #查看docker版本
```

![image-20241109084132658](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090841546.png)

这里我们很清晰的可以看到docker是一个C/S架构的模式。客户端是我们的命令行操作，服务端是一个守护进程。

```
docker info  #查看docker基本信息
```

我们可以通过**docker info**看到机器存放docker镜像的地址，也可以看到docker仓库的地址。

![image-20241109084243938](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090842844.png)

# docker入门命令

## 搜索镜像

**docker search**

```
docker search centos #从docker hub中搜索docker名为centos的镜像
```

![image-20241109084351402](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090843484.png)

表头：

name：代表此镜像的名称

description：此镜像的描述

stars：下载次数

official：是否由官方提供（官方提供可放心下载，可以基于此镜像做自己的镜像）

## 拉取镜像

**docker pull**，默认是拉取docker hub上搜索到的最新版本（第一个）

```
docker pull centos
```

![image-20241109084641744](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090846012.png)

注意，如果这里报错，TLS handshake timeout，那就是网络原因导致超时，尝试多pull几次。下面介绍配置镜像加速。

**使用阿里云docker镜像加速器。**

地址:**https://cr.console.aliyun.com**的控制台，使用支付宝账号登录，左侧加速器帮助页面会为你显示**独立的加速地址**，这个加速地址每个人的都不同。

![image-20241109084740924](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090847762.png)

**可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器。
把自己的专属加速地址放到下面的地址改一下，写入文件就可以了。**

```
{
  "registry-mirrors": ["https://eu5rxjvf.mirror.aliyuncs.com"]
}

systemctl daemon-reload  #启动配置
systemctl restart docker  #重启docker服务

```

配置好了之后，我们使用之前学的命令，docker info查看一下是否新增了阿里云的地址。

![image-20241109084837242](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090848140.png)

## 查看镜像

```
docker images #查看已下载镜像
```

![image-20241109084937217](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090849416.png)

## 使用U盘的方式导入镜像

```
docker load -i /root/docker-centos-httpd.tar
```

另外提一下，还有一种直接下载其他站点镜像的方法，命令如下：

```
docker pull hub.c.163.com/library/tomcat:latest
```

注：docker镜像相当于，对程序+程序依赖的库直接打包（后期详细解释）。

# Docker平台的基本使用方法

## 帮助命令

```
docker version #显示docker详细信息
docker info #显示docker的系统信息，包括镜像和容器的数量
docker --help #docker帮助命令手册
```

## 镜像命令

```
docker images #查看所有本地主机的镜像
docker search 镜像名 #搜索镜像
docker pull 镜像名 [标签] #下载镜像（如果不写tag，默认是latest）
docker rmi 镜像名 [标签] #删除镜像 
docker tag 镜像名:版本 新镜像名:版本 #复制镜像并且修改名称
docker commit -a "xxx" -c "xxx" 镜像ID 名字:版本 #提交镜像
-a：提交的镜像作者
-c：使用dockerfile指令来创建镜像
-m：提交时的说明文字

docker load -i /xxx/xxx.tar #导入镜像
docker save -o /xxx/xxx.tar #保存一个镜像为一个tar包
```

## 容器命令

```
docker run [可选参数] image #启动容器（无镜像会先下载镜像）
#参数说明
--name = "Name" 容器名字
-c 后面跟待完成的命令
-d 以后台方式运行并且返回ID，启动守护进程式容器
-i 使用交互方式运行容器，通常与t同时使用
-t 为容器重新分配一个伪输入终端，也即启动交互式容器
-p 指定容器端口 -p 容器端口:物理机端口 #映射端口
-P 随机指定端口
-v 给容器挂载存储卷

docker build #创建镜像 -f：指定dockerfile文件路径 -t：镜像名字以及标签
docker logs 容器实例的ID #查看容器日志
docker rename 旧名字 新名字 #给容器重新命名
docker top 容器实例的ID #查看容器内进程
docker ps -a #列出所有容器（不加-a就是在运行的）
docker rm 容器实例的ID #删除容器（正在运行容器不能删除，除非加-f选项）
docker kill 容器实例的ID #杀掉容器：强制停止当前容器
docker history 容器实例的ID #查看docker镜像的变更历史
docker start 容器实例的ID #启动容器
docker restart 容器实例的ID #重启容器
docker stop 容器实例的ID #停止正在运行的容器
docker attach /docker exec -it /bin/bash容器实例的ID #同为进入容器命令，不同的是attach连接终止会让容器退出后台运行，而exec不会。并且，docker attach是进入正在执行的终端，不会启动新的进程，而docker exec则会开启一个新的终端，可以在里面操作。
docker image inspect 容器名称：容器标签 #查看容器内源数据
docker cp 容器ID：容器内路径 目的主机路径 #从容器内拷贝文件到主机（常用）或者从主机拷贝到容器（一般用挂载）
exit #直接退出容器
ctrl + P + Q #退出容器但是不终止运行
```

# 实战测试：部署Nginx

## 搜索镜像

```
docker search nginx
```

## 下载镜像

```
docker pull nginx
```

未指定nginx则直接下载最新版本

## 查看镜像

```
docker images
```

## 启动容器

```
docker run -d -name nginx01 -p 80:80 nginx
```

返回容器实例ID

## 查看容器

```
docker ps #显示正在运行容器
```

## 测试访问

```
curl 127.0.0.1:80
```

## 进入容器修改页面

```
docker exec -it 容器ID /bin/bash
```

whereis 是一个搜索文件的小命令，不如find好用但是简洁

![image-20241109093015996](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090930953.png)

## 外网访问

（注意，外网IP需要在云平台打开端口，由于作者偷懒映射的80，所以没有去打开，如果是其他端口，就要去打开。）

## 实战总结

```
docker run [可选参数] image 命令 #启动容器（无镜像会先下载镜像）
#参数说明
--name = "Name"   容器名字
-c   后面跟待完成的命令
-d   以后台方式运行并且返回ID，启动守护进程式容器
-i   使用交互方式运行容器，通常与t同时使用
-t   为容器重新分配一个伪输入终端。也即启动交互式容器
-p   指定容器端口    -p 容器端口:物理机端口  映射端口
-P   随机指定端口
-v   给容器挂载存储卷
```

大家注意-i 、 -t 、 -d这几个参数。一般it连用表示给我一个可以操作的前台终端。第二个呢就是id，以后台守护进程的方式运行容器。这样，我们就可以总结出两种运行容器的命令模式。

```
第一种：交互方式创建容器，退出后容器关闭。
docker run -it 镜像名称:标签 /bin/bash

第二种：守护进程方式创建容器。
docker run -id 镜像名称:标签
通过这种方式创建的容器，我们不会直接进入到容器界面，而是在后台运行了容器，
如果我们需要进去，则还需要一个命令。
docker exec -it  镜像名称:标签  /bin/bash
通过这种方式运行的容器，就不会自动退出了。

```

# 镜像原理

## 镜像是什么

```
镜像是一种轻量级的、可执行的独立软件包。用来打包软件运行环境和基于运行环境的开发软件，它包含运行某个软件所需要的内容，包括代码、运行时、库、环境变量和配置文件。
```

## Docker镜像加载原理

**UnionFS（联合文件系统）**

UnionFS（联合文件系统）：分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层一层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像，可以制作各种各样的应用镜像。

特性：一次同时加载多个文件系统，但是从外面开起来，只能看一个文件系统，联合加载会把各层文件系统叠加起来，最终的文件系统会包含所有的底层文件和目录。

**Docker镜像加载原理**

docker的镜像实际上是由一层一层的文件系统组成，这种层级关系就叫UnionFS。
bootfs（boot file system）主要包括bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就在内存中了，此时内存的使用权由bootfs转交给内核，此时系统会卸载bootfs。

rootfs（root file system），在bootfs之上。包含的就是典型Linux系统的/dev, /proc, /bin, /etc等等标准文件。rootfs就是各种不同的操作系统发行版本，比如Ubuntu、CentOS等。

![image-20241109094809533](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090948534.png)

对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令、工具和程序即可，因为底层直接用Host的kernel，自己只需要提供rootfs即可。由此可见不同的Linux发行版本，bootfs基本上是一致的，rootfs会有差别，所以不同的发行版可以公用bootfs，这也是一个镜像仅有几百MB的原因。

## 分层理解

这里我用docker pull nginx命令，下载来一个镜像给大家看看，框起来的是不是一层一层下载的。

![image-20241109095050167](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090950086.png)

那么docker为什么会使用这种方法呢？最大的好处，就是资源共享。比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需要在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有容器提供服务了，而且镜像的每一层都可以被共享。

我们通过docker image inspect ngixn:latest查看一下。

![image-20241109095103556](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090951478.png)

这里给大家举个栗子：
所有的docker镜像都起始于一个基础镜像层，当进行修改或者增加新的内容时，就会在当前镜像层之上，创建新的镜像层。假如基于Ubuntu Linux 16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加Python包，就会在基础镜像之上创建第二个镜像层；如果继续添加安全补丁，就会创建第三个镜像层。如下图：

![image-20241109095122869](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090951711.png)

在添加额外的镜像的同时，镜像始终是当前所有镜像的组合。比如我们在添加第三层安全补丁的时候，Ubuntu和Python视为一个镜像层，在此基础上再添加安全补丁镜像层。
docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层就是我们所说的容器层，容器之下都叫镜像层。

## 提交镜像

#### 1、命令。

```c
docker commit 提交容器成为一个新的副本
docker commit -m="提交的描述信息"  -a="作者"  容器id  目标镜像名:[TAG]
12
```

#### 2、实验。

1、下载一个默认的tomcat，这里作者已经下载好了，就不用再下载了。

![image-20241109095514827](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090955686.png)

2、启动tomcat。
docker run -itd -p 8080:8080 tomcat:latest /bin/bash
然后进入此容器
docker exiec -it [容器ID] /bin/bash

![image-20241109095530220](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090955061.png)

3、默认tomcat镜像的webapp网页文件里是没有东西的，我们要从webapps.dist中把它拷贝出来。

![image-20241109095553716](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090955158.png)

4、打开8080端口，在浏览器访问tomcat docker。

![image-20241109095611584](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090956509.png)

5、提交镜像。
**docker commit -a=“This my create tomcat” -m=“add webapps app” 81 tomcat02:1.0**

![image-20241109095628013](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411090956740.png)

这里我们就制作了我们的第一个镜像，之后我还会教大家怎么将镜像发布到docker hub上。

# Docker容器数据卷

## 容器数据卷介绍

docker容器在产生数据的时候，如果不通过docker commit生成新的镜像，使得数据作为镜像的一部分保存下来，那么当容器删除之后，数据自然而然的也会消失。为了能保存数据，容器中引用了**数据卷**的概念。

## 作用以及特点

卷就是目录或者文件，存在一个或者多个容器之中，由docker挂载到容器，但是不属于联合文件系统，因此能够绕过Union File System提供一些用于持续存储或者共享数据的特性。

卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此docker不会再容器删除时删除其挂载的数据卷。

它还存在以下几种特点：

1、数据卷可在容器之间共享或者重用数据。
2、卷中的更改可以直接生效。
3、数据卷中的更改不会包含在镜像的更新中。
4、数据卷的生命周期一直持续到没有容器使用它为止。

## 使用数据卷

方式一：直接使用命令来挂载 ， -v

```
docker run -it -v 主机目录:容器目录 /bin/bash
```

我们在创建容器之前，先看看挂载路径上有没有test01这个目录，可以看到，是没有的。执行命令之后进入到容器内，我们ls看一下容器的home目录，是空的。

```
docker run -it -v /home/test01:/home centos /bin/bash
```

![image-20241109100315259](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091003182.png)

另外打开一个终端，cd /home目录，这下我们发现多出来了一个test01目录，这个test01目录，就是我们刚刚启动的容器内部的home目录，并且，此时这两个目录是同步的状态，我们在home目录中写入任何新的文件，都会同步到主机home目录下的test01目录。

![image-20241109100338099](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091003917.png)

我们在这里测试一下，echo进去一个a.txt文件。

![image-20241109100349954](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091003862.png)

然后来到宿主机上，看一眼是不是test01目录下也出现了a.txt。(双向绑定)

![image-20241109100400082](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091004005.png)

当然，我们可以使用更简单的方法查看是否挂载成功，大家还记得是那条命令吗？没错，是docker inspect 容器ID。我们找到这个Mounts，它代表着挂载，type是类型（绑定），source是源（/home/test01），也就是把什么挂载到哪里。destination（home）就是挂载的目标路径了。

![image-20241109100412883](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091004653.png)

## 具名挂载与匿名挂载

```
docker volum ls #查看所有卷的情况。
```

##### 1、匿名挂载

我们首先使用匿名挂载的命令启动一个容器。

```
docker run -d -P --name=nginxt01 -v /etc/nginx nginx
```

然后使用刚刚教给大家的新武器查看卷。

![image-20241109100853265](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091008100.png)

这里84开头的那一长串就是挂载到宿主机的名字。
我们继续追查下去。这里教给大家一个很简单的命令，less。如果输出的信息太多了，我们找不到，就可以这样使用
cmd | less 栗子： docker inspect 84（容器ID） | less
然后输入/name, name是你想查到的内容，就可以很轻松的找到啦。
大家仔细看一下，是不是/etc/nginx就是叫84开头的那一长串，挂载到了我/var/lib…路径下，我们复制这个路径继续去查看。
![image-20241109100909834](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091009779.png)

怎么样，是不是在我们的宿主机就发现了这样的一个文件呢？这就是所谓的匿名挂载！是不是很简单。

![image-20241109100926282](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091009166.png)

##### 2、具名挂载。

具名挂载就很简单了，跟我们之前演示的指定路径挂载很相似，这里给大家简单地演示一下。

同样，我们使用具名挂载的方式启动一个容器。

```
docker run -d -P --name=nginxt02 -v jumingguazai:/etc/nginx nginx
```

docker volume ls 查看卷

![image-20241109100953098](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091009877.png)

docker inspect ID | less 找到挂载点。

![image-20241109101005698](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091010445.png)

我们再复制一下路径，找到nginx的配置文件。

![image-20241109101016882](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091010732.png)

这就是具名挂载。

```
如何确定是具名挂载还是匿名挂载：
-v  容器内路径               #匿名挂在
-v  卷名：容器内路径          #具名挂在
-v  /宿主机路径：容器内路径    # 指定路径挂载
```

tips：

```
通过 -v 容器内路径  :ro   rw   可以改变读写权限
ro  readonly   #只读
rw  readwrite  #可写可读
例： docker run -d --name nginx01 -v test01:/etc/nginx:ro nginx
    docker run -d --name nginx01 -v test01:/etc/nginx:rw nginx
```



# Dockerfile

## 什么是Dockerfile

Dockerfile是一个创建镜像所有命令的文本文件，包含了一条条指令和说明, 每条指令构建一层,，通过docker build命令，根据Dockerfile的内容构建镜像，因此每一条指令的内容, 就是描述该层如何构建。有了Dockefile,，就可以制定自己的docker镜像规则,只需要在Dockerfile上添加或者修改指令,，就可生成docker 镜像。

## Dockerfile构建过程

dockerfile的关键字建议使用大写，它是从上往下按照循序执行的，在dockerfile中，#代表注释。我们可以通过这个脚本来生成镜像，脚本中的每一个命令，都是一层镜像。

在这里我们来整理一下docker容器、dockerfile、docker镜像的关系：

**dockerfile**是面向开发的，发布项目做镜像的时候就要编写dockerfile文件。
**dockerfile**：构建文件，定义了一切的步骤，源代码。
**dockerImanges**：通过dockerfile构建生成的镜像，最终发布和运行的产品。
**docker容器**：容器就是镜像运行起来提供服务的。

## Dockerfile 指令选项。

```
Dockerfile 指令选项:

FROM                  #基础镜像 。 （centos）
MAINTAINER            #镜像的作者和邮箱。（已被弃用，结尾介绍代替词）
RUN                   #镜像构建的时候需要执行的命令。
CMD                   #类似于 RUN 指令，用于运行程序（只有最后一个会生效，可被替代）
EXPOSE                #对外开放的端口。
ENV                   #设置环境变量，定义了环境变量，那么在后续的指令中，就可以使用这个环境变量。
ADD                   # 步骤：tomcat镜像，这个tomcat压缩包。添加内容。
COPY                  #复制指令，将文件拷贝到镜像中。
VOLUME                #设置卷，挂载的主机目录。
USER                  #用于指定执行后续命令的用户和用户组，
                       这边只是切换后续命令执行的用户（用户和用户组必须提前已经存在）。
WORKDIR               #工作目录（类似CD命令）。
ENTRYPOINT            #类似于 CMD 指令，但其不会被 docker run 
                       的命令行参数指定的指令所覆盖，会追加命令。
ONBUILD               #当构建一个被继承Dokcerfile，就会运行ONBUILD的指令。出发执行。


注意：CMD类似于 RUN 指令，用于运行程序，但二者运行的时间点不同:
CMD 在docker run 时运行。
RUN 是在 docker build。
作用：为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。
CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。
如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。

LABEL（MAINTALNER已经被弃用了，目前是使用LABEL代替）
LABEL 指令用来给镜像添加一些元数据（metadata），以键值对的形式，语法格式如下：
LABEL <key>=<value> <key>=<value> <key>=<value> ...
比如我们可以添加镜像的作者：
LABEL org.opencontainers.image.authors="runoob"

```

这里带大家回顾一下**docker history**命令。接下来我们就要用dockfile制作属于自己的镜像了。

![image-20241109101535601](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091015494.png)

通过这个命令，我们就能看到dockerfile制作镜像所执行的步骤，也就可以知道这个镜像是怎么制作的了。

# 实战测试：制作镜像并且发布到外网

## 注册docker hub账号

网址：https://hub.docker.com/

需翻墙

![image-20241109101811066](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091018891.png)

## 服务器上使用命令行登录

命令

```
docker login -u [账号名字]   #登陆命令
docker out                  #退出命令
docker push 账号/容器名字：版本号
```

![image-20241109101915754](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091019603.png)

看到Login Succeeded，就表示我们登陆成功了。

## 构建镜像

### 1、创建工作目录。

```
mkdir dockerfile
cd dockerfile
ls
```

### 2、编写dockerfile。

首先，我们知道官方默认的镜像，比如centos镜像里面，没有vim、ipconfig等命令，我们就基于此，创建此镜像。

```
vim mydockerfile
```

```
FORM centos
MAINTAINER ydk<123@qq.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH
RUN yum -y install vim-enhanced
RUN yum -y install net-tools
EXPOSE 80
CMD echo #MYPATH
CMD echo "------------END-------------"
CMD /bin/bash
```

![image-20241109102113596](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091021344.png)

### 3、构建dockerfile

docker build 命令：

```
docker build -f mydockerfile-t mycentos:1.0 .
```

![image-20241109102225635](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091022667.png)

到这里，我们就制作好了我们自己的镜像，虽然它并没有什么用。
这里我们再启动我们自己制作的镜像，进去看看我们写的dockerfile都生效了没有。
注：不加标签**默认是latest**，所以docker run的时候要带上镜像标签。

![image-20241109102249629](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091022379.png)

同时，我们可以用**docker history**命令来进一步验证dockerfile的构建过程。

![image-20241109102311849](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091023699.png)

### 4、推送镜像至docker hub。

官方文档要求，我们推送的镜像名字必须是**YOUR_DOCKER_HUB_ID/XXXX**，所以我们需要给镜像换一个名字

```
docker tag  mycentos/1.0 自己的账号名字/mytomcat 
docker push 自己的账号名字/mytomcat 
```

镜像有点大，所以请耐心等待一下。等了几分钟之后，我们登陆docker hub就可以看到我们刚刚推送上去的镜像啦，这个镜像可是全世界人民都看得到的哦，是不是有点小激动呢！

![image-20241109102428729](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091024451.png)

# Docker网络

## 本机网络理解

我们使用ifconfig可以看到三组网络。
首先是docker0，这是我们本节的重点，docker的网络。之后是eth0，本机的外网地址。lo口，本地环回地址，可以代表localhost。

![image-20241109102645829](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091026755.png)

关于docker0呢，其实就是一个叫docker0的虚拟网桥。我们使用brctl命令来查看一下。（没有这个命令的下载**yum -y install bridge-utils**）

```
brctl show 
```

![image-20241109102717667](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091027493.png)

## 清空本机docker环境

```
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images -aq)
```

## veth-pair技术

什么是veth-pair技术？要理解它，我们首先来启动两个tomcat容器。

```
docker run -d -P --name=tomcat01 tomcat:7
docker run -d -P --name=tomcat02 tomcat:7
提示：选择tomcat7是因为这个镜像包含了ip addr 等常用命令！
```

启动机器之后，我们查看容器ip，通过容器的ip 去ping宿主机ip，发现是通的。

```
docker exec -it tomcat01 ip addr
```

ip addr == ip a ：可以查看网卡的ip、mac等。即使网卡处于down状态，也能显示出网卡状态，但是ifconfig查看就看不到。

![image-20241109102910627](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091029508.png)

```
ping 172.17.0.3
```

![image-20241109103155687](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091031570.png)

理解：我们每启动一个docker容器，docker就会给docker容器分配一个ip，宿主机就会多一个docker的网卡。而安装docker之后，会产生一个叫docker0的网卡，这里使用的就是**veth-pair技术**。

使用ip addr命令，查看我们的网卡。

![image-20241109103302489](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091033282.png)

我们发现多出来了两个网卡，到了这里，你已经知道这两张网卡是那里来的了吧。没错，是启动容器之后产生的！我们回过头来查看我们在启动的容器IP，就会很清晰的发现，这个网卡是成对存在的！容器内的64对应着宿主机的65，容器内的66对应宿主机的67。

什么是veth-pair？
veth-pair 就是一堆的虚拟设备接口，他们都是成对出现的，一端连接着协议，一端连接着彼此。使得它充当了一个桥梁的作用。

![image-20241109103459167](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091035051.png)

## docker网络详解

我们来绘制一个简单的网络模型，这样veth-pair的作用就清晰明了了。

![image-20241109103559520](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091036307.png)

不难看出，tomcat01和tomcat02是共用的同一个路由器，即docker0。所有的容器在不指定网络的情况下，都是docker0路由的，docekr会给我们的容器分配一个默认IP。
docker网络就是下面这个网络模型所描述的。（docker所有的网络接口都是虚拟的，虚拟的转发效率高）

![image-20241109103819227](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091038314.png)

## docker网络模式

### 1、docker网络模式有以下几种：

```
Host：容器不会虚拟出自己的网卡，配置主机的IP等,而是使用宿主机的IP和端口

Container: 创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP。（一般不用）

None: 该模式关闭了容器的网络功能。（一般不用）

Bridge：默认为该模式（桥接，自己创建也是用它），此模式会为每一个容器分配，设置IP等，并将容器连接到一个docker0 的虚拟网桥，通过docker 0 网桥以及iptables nat 表配置与宿主机通信。
```

```
docker network ls   #列出docker网卡
```

### 2、创建自定义网络的容器：

```
我们直接启动命令， --net bridge，就是docker0（默认）
docker run -d -P --name=tomcat01 --net bridge tomcat

docker0特点：默认，域名不能访问，--link不建议使用
```

下面我们自己来创建一个bridge。

```
docker network create --driver bridge --subnet 192.168.0.0/24 --gateway 192.168.0.1 testnet

docekr network ls
```

![image-20241109104131480](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091041665.png)

这里在教大家一条命令：

```
docker network inspect 网卡名字  #查看网卡详细信息
```

![image-20241109104222984](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091042945.png)

### 3、发布两个在自己创建的网络里的容器。

```
docker run -d -P --name=tomcat01-net --net=testnet tomcat:7
docker run -d -P --name=tomcat02-net --net testnet tomcat:7
```

然后使用docker network inspect testnet，就可以看到刚才创建的这两个容器的IP了。

![image-20241109104354135](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091043129.png)

还记得我们前面说的docker0的缺点之一，不能通过域名访问吗？而我们自定义的网络，就修复了这个功能！

```
docker exec -it tomcat01-net ping -c 3 IP
docker exec -it tomcat01-net ping -c 3 tomcat02-net

提示，ping -c可以自定义ping的次数
```

![image-20241109104435476](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202411091044426.png)