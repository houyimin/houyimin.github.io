---
title: 本地配置多个ssh key
date: 2018-07-23 16:23:53
tags:
 - github
categories:
 - github
---

1、为gitlab生成ssh key
``` bash
ssh-keygen -t rsa -C 'yourEmail@xx.com' -f ~/.ssh/gitlab-rsa

```

2、为github生成ssh key
``` bash
ssh-keygen -t rsa -C 'yourEmail2@xx.com' -f ~/.ssh/github-rsa
```

3、在~/.ssh目录下新建名称为config的文件（无后缀名）。用于配置多个不同的host使用不同的ssh key，内容如下：

``` bash
# gitlab
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitlab-rsa
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github-rsa
  ​
# 配置文件参数
# Host : Host可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件
# HostName : 要登录主机的主机名
# User : 登录名
# IdentityFile : 指明上面User对应的identityFile路径
```

4、按照上面的步骤分别往gitlab和github上添加生成的公钥gitlab-rsa.pub和github-rsa.pub



5、查看ssh key  
cat ~/.ssh/github-rsa.pub

6、测试是否连接
ssh -T git@github.com