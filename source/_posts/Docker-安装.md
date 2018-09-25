---
title: Docker 安装
date: 2018-09-10 16:21:45
tags:
 - Docker
categories:
 - Docker
---

## 一、win7、win8 系统

win7、win8 等需要利用 docker toolbox 来安装，国内可以使用阿里云的镜像来下载，
下载地址：http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/


下载安装完成后，点击 Docker QuickStart 图标来启动 Docker Toolbox 终端。


``` bash
docker run hello-world

```

## 二、 CentOS 6.8 安装 Docker

CentOS 7 的内核一般都是3.10的，而CentOS 6.X 的内核一般都是2.6，在2.6的内核下，Docker运行会比较卡，所以一般会选择升级到3.10版本。

### 安装docker-io
``` bash
[root@localhost ~]# yum install docker-io
```


## 三、使用 yum 安装（CentOS 7下）

  Docker 要求 CentOS 系统的内核版本高于 3.10  。

通过 uname -r 命令查看你当前的内核版本	

``` bash
	[root@runoob ~]# uname -r  3.10.0-327.el7.x86_64
```

参考链接： [使用 yum 安装（CentOS 7下）](http://www.runoob.com/docker/centos-docker-install.html)
	

## 配置 Docker 加速器

参考链接： [Docker 加速器）](https://www.daocloud.io/mirror#accelerator-doc)

{% cq %} 
 ### [参考链接]
 [Docker 教程](http://www.runoob.com/docker/docker-tutorial.html)
	
 [CentOS 6.8 安装 Docker](https://blog.csdn.net/jeffleo/article/details/70904368)	
{% endcq %}