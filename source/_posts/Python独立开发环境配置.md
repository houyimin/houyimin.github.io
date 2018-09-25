---
title: Python独立开发环境virtualenv配置
date: 2017-07-03 17:38:05
tags:
 - Python
categories:
 - Python
---


 ### 什么是virtualenv
 Virtualenv是一个用来创建独立的Python环境的包。



你可以为每个项目建立不同的/独立的Python环境，你将为每个项目安装所有需要的软件包到它们各自独立的环境中。


## 安装与使用virtualenv
``` bash
pip install virtualenv
```

virtualenv安装完毕后，可以通过运行下面的命令在项目目录myproject创建独立的python环境
``` bash
mkdir myproject
cd myproject
virtualenv --no-site-packages myproject
```
参数--no-site-packages代表已经安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境。

以上代码安装的是系统默认的python版本，如果需要安装指定版本的python，比如安装python2.7可以用以下代码。
``` bash
mkdir myproject
cd myproject
virtualenv --no-site-packages --python=C:\Python27\python.exe myproject 
```

## 通过下面的命令激活这个virtualenv：activate为激活文件
``` bash
source bin/activate
```
windows执行
``` bash
cd Scripts
activate
```

``` bash
(myproject) D:\houyimin\learnpython\myproject\Scripts>
```
注意到命令提示符变了，有个(myproject)前缀，表示当前环境是一个名为myproject的Python环境。


## 运行下面的命令退出virtualenv环境。
``` bash
deactivate  
```

virtualenv拷贝了Python可执行文件的副本，并创建一些有用的脚本和安装了项目需要的软件包，你可以在项目的整个生命周期中安装/升级/删除这些包。 它也修改了一些搜索路径，例如PYTHONPATH，以确保：
当安装包时，它们被安装在当前活动的virtualenv里，而不是系统范围内的Python路径。
当import代码时，virtualenv将优先采取本环境中安装的包，而不是系统Python目录中安装的包。
