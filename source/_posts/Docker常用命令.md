---
title: Docker常用命令
date: 2018-09-14 18:12:23
tags:
 - Docker
categories:
 - Docker
---

## 运行一个应用

``` bash
docker pull nginx

docker run -d  -p 8080:80 --name='nginx1' -v /local/html:/usr/share/nginx/html  nginx

```

-d:让容器在后台运行。
-p:将容器80端口映射到宿主机8080端口。

## 查看正在运行的容器

``` bash
docker ps

```

## 使用 docker port 可以查看指定 （ID或者名字）容器的某个确定端口映射到宿主机的端口号

``` bash
docker port 7a38a1ad55c6

```

## 查看WEB应用程序日志

``` bash
docker logs -f 7a38a1ad55c6

```

## 检查WEB应用程序,使用 docker inspect 来查看Docker的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。

``` bash
docker inspect determined_swanson

```



## 停止、启动、删除容器

``` bash
docker stop determined_swanson   
docker start determined_swanson
docker rm determined_swanson

```

## docker exec ：在运行的容器中执行命令

-d :分离模式: 在后台运行

-i :即使没有附加也保持STDIN 打开

-t :分配一个伪终端

### 在容器mynginx中以交互模式执行容器内/root/runoob.sh脚本
``` bash
docker exec -it mynginx /bin/sh /root/runoob.sh

```
### 在容器mynginx中以交互模式执行容器内/root/runoob.sh脚本
``` bash
 docker exec -i -t  mynginx /bin/bash

```