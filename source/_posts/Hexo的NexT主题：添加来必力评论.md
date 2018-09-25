---
title: Hexo的NexT主题：添加来必力评论
date: 2017-05-26 13:33:23
tags:
 - Hexo
categories:
 - Hexo
---
{% cq %} 
 ### [来必力](https://livere.com)是什么？
 使用社交网站账户登录，免去注册过程。
 提高用户的参与和沟通意愿。
 管理/删除我的评论内容。
 提供管理页面，管理网站文章及评论内容。
{% endcq %}

## 安装来必力 
### 1 、登陆[来必力](https://livere.com)网站注册
### 2 、在后台的代码管理页面 查看一般网站的安装代码,复制data-uid的值
![安装代码](/uploads/1495780206.png)
### 3、打开next主题的配置文件 \themes\next\_config.yml，找到 livere_uid 设置为该值即可。
``` bash
# Support for LiveRe comments system.
# You can get your uid from https://livere.com/insight/myCode (General web site)
livere_uid: 这里填写data-uid的值
```
{% note success %} 基本设置 {% endnote %}

### 1 、注意需要在来必力后台管理的设置中添加网站名称和网站的URL，可根据情况选择要显示的SNS第三方登陆按钮。
![安装代码](/uploads/20170526151406.png)
### 2、评论提醒：在评论提醒开启登记新评论时，接受提醒，填写接受提醒的邮箱地址以及选择提醒周期。 	
### 3、评论管理中可针对接收到的评论进行管理。	

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=86384&auto=0&height=66"></iframe>



