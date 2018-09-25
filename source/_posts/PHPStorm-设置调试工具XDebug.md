---
title: PHPStorm 设置调试工具XDebug
date: 2017-06-07 19:31:28
tags:
 - PHP
categories:
 - PHP
---


## 安装XDebug
### 如果使用的PHP开发环境是phpStudy，本身已经安装了XDebug扩展，只需在php.ini中开启即可。
``` bash
[XDebug]
xdebug.profiler_output_dir="D:\phpStudy\tmp\xdebug"
xdebug.trace_output_dir="D:\phpStudy\tmp\xdebug"
zend_extension="D:\phpStudy\php\php-5.5.38\ext\php_xdebug.dll"
xdebug.remote_enable = on
```
![安装代码](/uploads/1.jpg)

### 如果没有安装，可以到 [xdebug网站](https://xdebug.org/wizard.php) 下载相应版本的XDebug扩展php_xdebug.dll,只需将phpinfo的内容复制到输入框，第一种方式可以打印phpinfo()，复制网页内容，第二种方式在命令行将phpinfo的内容输出到文件中 php -i >phpinfo.txt ，复制phpinfo.txt的内容到输入框。最后点击按钮 Analyse my phpinfo() output 就会显示你要下载的版本。

![安装代码](/uploads/2.jpg)

## 设置PHPStorm

### 一、 添加服务器信息
  ### 1、 进入File>Settings>Languages & Frameworks>PHP>Servers，根据自己的域名进行设置，www.xywang.net是我配置的要进行debug的域名。
![安装代码](/uploads/server.jpg)
### 2、进入File>Settings>Languages & Frameworks>PHP>Debug>DBGp Proxy 填写： 
``` bash
IDE key: phpStorm 
host: www.xywang.net 
port: 80 
```
![安装代码](/uploads/dbgp.jpg)

### 3、最后点菜单栏的Run>Edit Configurations… 在弹出的窗口中添加一个调试配置：点击左上角加号，选择PHP Web Application，选择刚才配置的Server，选择默认浏览器为chrome
![安装代码](/uploads/chrome.jpg)

### 二、安装chrome调试插件Xdebug helper

### 安装好后在浏览器右上角会有一个灰色的小甲虫，右键选项中设置IDE key为PhpStorm,调试时要选择Debug选项成为绿色。

![安装代码](/uploads/jiacong.jpg)
![安装代码](/uploads/Xdebug_helper.jpg)

## 开始调试
### 调试时，在phpstrom中设置好断点，并点击右上角开始监听调试按钮，最后刷新网页即可进行调试。

![安装代码](/uploads/20170629154856.png)


