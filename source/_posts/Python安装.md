---
title: Python安装
date: 2017-10-12 11:12:23
tags:
 - Python
categories:
 - Python
---
## 安装Python 3.X

### 在Windows上安装Python
根据你的Windows版本（64位还是32位）从Python的官方网站[www.python.org](https://www.python.org)下载，然后，运行下载的EXE安装包

### 在Linux上安装Python


打开WEB浏览器访问[www.python.org/download](http://www.python.org/download/)
选择适用于Unix/Linux的源码压缩包。
这里，我选择的版本是 3.5.2 

``` bash
下载
#wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz
解压压缩包
#tar -zxvf Python-3.5.2.tgz
进入目录
#cd Python-3.5.2/
添加配置
#./configure
编译程序
#make
执行安装
#make install
```
执行以上操作后，Python会安装在 /usr/local/bin 目录中，Python库安装在/usr/local/lib/pythonXX，XX为你使用的Python的版本号。

### 验证 安装成功以后，就可以查看 Python 的版本了：
``` bash
# python -V
Python 2.7.5
# python3 -V
Python 3.5.2
```

### 设置 3.x 为默认版本
查看 Python 的路径，在 /usr/bin 下面。可以看到 python 链接的是 python 2.7，所以，执行 python 就相当于执行 python 2.7。
``` bash
# ls -al /usr/bin | grep python
-rwxr-xr-x.  1 root root      11216 12月  1 2015 abrt-action-analyze-python
lrwxrwxrwx.  1 root root          7 8月  30 12:11 python -> python2
lrwxrwxrwx.  1 root root          9 8月  30 12:11 python2 -> python2.7
-rwxr-xr-x.  1 root root       7136 11月 20 2015 python2.7

将原来 python 的软链接重命名：
# mv /usr/bin/python /usr/bin/python.bak

将 python 链接至 python3：
# ln -s /usr/local/bin/python3 /usr/bin/python

这时，再查看 Python 的版本：
# python -V
Python 3.5.2
输出的是 3.x，说明已经使用的是 python3了。
```


 ### [参考链接]
 [Python 环境搭建](http://www.runoob.com/python/python-install.html)
 [Linux 升级 Python 至 3.x](http://blog.csdn.net/liang19890820/article/details/51079633)	
