---
title: hexo多终端发布博客
tags:
 - Hexo
categories:
 - Hexo
---
> 网站的部署其实就是生成静态文件，hexo下所有生成的静态文件会放在public/文件夹中，所谓部署deploy其实就是 
将public/文件夹中内容上传到git仓库

## 换了电脑怎么办？

### 在现有的guthub.io的repository下创建一个分支来管理

1. 克隆仓库到本地

``` bash
git clone git@github.com:XXX.github.io.git
```

2. 删除文件夹里除了.git的其他所有文件
3. 把你的blog文件夹内的所有文件全部复制到XXX.github.io/下
4. 创建一个叫hexo（或者blog，名字随意）的分支，并切换到这个分支

``` bash
git checkout -b hexo
```

5. 添加文件，推送到远程仓库


``` bash
# 添加所有文件到暂存区
git add --all

#提交
git commit -m ""

#推送hexo分支的文件到github仓库
git push --set-upstream origin hexo

```

**最后的效果就是仓库中的master放到是生成博客页面的文件（也就是blog/public/下的的文件），分支hexo中存放的就是我们备份的必要的blog中的文件。**

**发布博客后，执行指令，将备份的文件推送到hexo分支**

``` bash

git add . #添加所有文件到暂存区
git commit -m "提交一篇博客"  #提交
git push origin hexo 推送hexo分支到github

```

**今后如果换电脑的话，配置好基本的环境，然后克隆hexo分支到本地,npm install 安装依赖**

``` bash

git clone -b hexo git@github.com:XXX.github.io.git

```

## 综上所述

**新建博客hexo new post "你好，hexo" ，然后去blog\source_posts 编辑文章。以后每次写完博客，通过hexo g，hexo d发布博客，然后通过git三部曲git add . ; git commit -m "注释" ; git push origin hexo更新备份github的hexo分支即可**


