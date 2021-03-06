---
title: vagrant技术
date: 2018-12-18 12:37:26
updated: 2018-12-18 12:37:26 
mathjax: false
categories: 
tags:
typora-root-url: Vagrant
typora-copy-images-to: Vagrant
top: 1
---

---
typora-root-url: vagrant
typora-copy-images-to: vagrant
---

# vagrant技术

## ①-常见问题描述

在项目交互部署的时候（往往开发和部署不是同一个团队做的，开发有专门的开发人员；部署一般都是运维部署），经常听到 “在我的电脑明明是好好的，为什么在你的电脑就不可以使用了呢? 是不是你的环境没有装好？”



之前我们一般是使用wamp环境在本地开发（习惯在windows下做开发），开发测试通过后，然后使用ftp、xshell等一定的工具把我们项目代码上传到线上服务器lamp环境上，这个时候一般会出现一些问题。例如Linux下和window下环境不一致，为了防止这种情况，一般我们都已在本地搭建一些lamp环境，但是这个lamp一般我们是使用本地的虚拟机软件（VMware、VirtualBox）搭建出来。

但是通过这些虚拟机软件搭建环境的时候，我们需要手工的去创建一个虚拟电脑，然后在虚拟电脑上进行环境搭建，但是这个过程往往是枯燥耗费时间的，甚至有的时候还安装不成功。



基于上面常出现的这两种情况，有人就提出了一个可行性的方案：

通过一定的软件来帮我们去管理我们的虚拟机软件，然后通过该工具直接的管理虚拟机软件。让虚拟机软件去执行对应的操作。例如我们需要按照一个操作，只需要使用该工具给虚拟机软件发送一个如下的命令：

```
> vagrant  box  add  CentOs65_x64  url
> vagrant  up 
```

这用的工具就是我们今天学习的**vagrant**工具。

**vagrant 是一个软件，专门帮我们管理虚拟机、或者操作系统镜像的，可以保证环境的一致。**

后期还会学习一个docker的技术来实现环境的一致。



 

## ②-什么是vagrant工具？

网站：<https://www.vagrantup.com/>

![img](wps12BE.tmp.jpg)

 

 

**Vagrant**是一个软件，可以自动化虚拟机的安装和配置流程。如果需要用Linux环境进行开发或学习，使用虚拟机无疑是最方便的选择。而Vagrant更进一步，可以让你通过编写一个`Vagrantfile`文件来控制虚拟机的启动、虚拟机网络环境的配置、虚拟机与主机间的文件共享。

这意味着，当你需要在多台机器间同步开发进度时，只需要同步Vagrantfile文件，就可以**保证各台机器拥有一致的开发环境**。另外，即便对于计算机小白用户，Vagrant也是一个利器。以前我们为了学习一门语言，必须先手动安装这门语言的编译环境。这期间的各种痛苦想必各位深有体会。有了Vagrant后，我们可以下载别人写好的`Vagrantfile`，然后运行`vagrant up`，vagrant就会自动下载虚拟机镜像，自动加载镜像并配置虚拟机，然后给我们一个即开即用的学习环境。

 

## ③-vagrant的依赖环境



由于vagrant是管理虚拟机软件的，那么自然需要==虚拟机==程序和==操作系统镜像==。前者常用的选择有**VirtualBox**和**VMWare**，后者则包括Ubuntu、FreeBSD、window7等等。Vagrant称前者为**provider**（提供商），称后者为**box**（安装的镜像文件，只是这个镜像文件可以理解成是已经做好的一个操作系统，不需要我们在手工的去安装，拿过来直接可以使用）。原则上，我们可以自由搭配provider和box，但由于VirtualBox开源且免费，Vagrant将其作为默认的provider。所以，一般会先安装VirtualBox，再安装Vagrant。

### 软件安装

1. 先安装virtualBox（4.3版本） ，全程下一步即可。

![1538187060443](1538187060443.png) 





2. 在安装vagrant（最新的），全程下一步即可。

![1538179806838](1538179806838.png)

 

### 注意网卡禁用

如果之前我们的电脑安装过VMware这个软件，这个软件安装之后需要在我们自己电脑上虚拟出来一些网卡，则我们为了防止可能影响我们的virtualbox，则我们先把VMware这些网卡全部禁用。

![1538187172549](1538187172549.png)



### 成功安装

1. virtualbox安装后，可以点击该软件，显示如下

![img](wps12D1.tmp.jpg) 

上面的这个软件就是一个虚拟机软件，我们可以使用它去创建很多的虚拟电脑，然后在虚拟电脑上面进行操作系统的安装，但是这个过程是很繁琐的，也是耗费时间的，没有任何的意义。完全没有必要自己尝试去装操作系统，或者说在安装之后，在该操作系统上面去搭建属于自己的开发环境，一般来说我们可以在安装完成该软件之后，在安装一个vagrant的软件帮我们去管理上面的虚拟机软件，然后通过vagrant去下载我们需要的操作系统，并且把操作系统直接导入上面的虚拟机里面去，而不需要自己去安装操作系统，搭建环境。



2. vagrant安装之后的测试命令，查看一下版本即可。

![img](wps12D2.tmp.jpg) 

如果使用 `vagrant -v` 可以查看到版本信息，代表vagrant安装是没有问题的

 

### Linux环境模拟

注意：由于vagrant最好是使用Linux的`cli`环境，所以最好装一个Linux的环境模拟，如 `git` 或 `cmder`

![img](wps12E3.tmp.jpg) 

安装成功后，鼠标右键即可看到如下

![img](wps12E4.tmp.jpg) 

点击后，出现如下的界面

![img](wps12F4.tmp.jpg) 

 

## ④-基本使用

### box选择

一般我们会从网上下载某个虚拟机的安装镜像到本地，文件名为`***.iso`，然后使用VirtualBox加载镜像并安装操作系统到虚拟机中，期间可能涉及到网卡、USB等虚拟硬件的配置。之后就可以正常的使用这个虚拟的系统了。

这一过程基本类似于使用安装盘在一台真实的电脑上安装系统。但Vagrant使用的镜像并不是待安装的系统镜像，而是从虚拟机中导出的、对已经安装配置好的操作系统的快照，以`.box`作为扩展名。

这类似于Windows中完整的系统备份所产生的镜像文件。`.box`文件不过是个压缩文件包，里面除了基础数据的镜像外，还包括一些开发环境。我们当然可以从最原始的安装镜像开始，一步步制作自己的`.box`镜像。但这一过程比较麻烦，也可以使用Vagrant官网提供了许多制作好的box文件。选择时注意box与provider的对应关系。

网站：https://app.vagrantup.com/boxes/search



本次课程使用自己下载的盒子进行操作

![1538189780340](1538189780340.png)

 

==注意：在导入的时候需要使用非中文的文件夹==



![1538187287539](1538187287539.png)



### 盒子添加-add

**我们介绍如何添加box文件到Vagrant的box管理系统中呢？**

 

1. 切换到盒子的目录，执行如下命令

   ```
   vagrant box add    盒子名称  盒子的绝对目录
   ```

   `盒子名称` ： 可自定义

   `盒子的绝对目录`  ： 需要使用正斜线

   **示例**

   ```
   >  vagrant box add centos64 C:/workspace/centos64/centos65_x64.box
   ```

   ![1538187404455](1538187404455.png)



### 初始化盒子-init和启动盒子-up

```
> vagrant init centos64

> vagrant up
```

执行 `vagrant init centos64 `命令的时候，这里的centos64是上面使用 `vagrant box add` 添加盒子时候指定的名称。


![1538187707232](1538187707232.png)

 

### vagrantfile的基本配置

1. 配置用户信息

![1538189548360](1538189548360.png)



2. 配置一下公共的网络 ， 和我们的自己的电脑(宿主机) 处于同一个网络。

![1538189533091](1538189533091.png)



3. 让vagrant重新读取配置文件

```
> vagrant reload
```

![1538189479973](1538189479973.png)





### 虚拟机连接工具-SSH

![1538189371457](1538189371457.png)



### 远程登录

使用git模拟环境进行远程登录

ssh命令登录

 ```
 > ssh root@127.0.0.1 -p 2222
 ```

会提示输入加密的密钥，点击确定，同时root的密码为admin88



#### vagrant ssh快速登录

当我们在导入盒子的目录下，执行如下的命令的时候，可以不输入任何端口和用户名以及IP的情况下，快速登录到linux操作系统

```
> vagrant ssh
```

执行这个命令的时候，去尝试读取VagrantFile文件里面的

```
config.ssh.username
config.ssh.password
```



### 退出登录

如果大家要退出执行，在linux下执行exit即可。

![1538190875110](1538190875110.png)



### 关闭虚拟机-halt

切换到盒子安装目录，执行如下命令

```
> vagrant halt
```

 ![1538191013605](1538191013605.png)



### 盒子卸载-destroy

进入盒子导入的目录，执行如下命令

```
> vagrant destroy
```



### 盒子导出

```
> vagrant package --base  centos65_default_1538191254489_16849  --out   C:/workspace/mycentos65.box
```

![1538191932374](1538191932374.png)





## 参考阅读

[**laravel开发的homestead环境搭建**](https://laravelacademy.org/post/9530.html) 





