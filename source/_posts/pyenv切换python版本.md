---
title: pyenv切换python版本
date: 2018-07-26 22:19:05
tags:
 - Python
categories:
 - Python
---

1、查看版本
``` bash
pyenv install --list 

```

指定版本安装
``` bash
pyenv install 3.4.3 -v

```


2.安装后，记得要更新
``` bash
 pyenv rehash
```
3.查看已安装版本
``` bash
pyenv versions
```
* system (set by /home/seisman/.pyenv/version)
3.4.3

4.指定版本
``` bash
pyenv global 3.4.3
```

5.切回原来版本
``` bash
pyenv global system
```