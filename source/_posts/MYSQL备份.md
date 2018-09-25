---
title: MYSQL备份
date: 2018-04-09 14:20:57
tags:
 - Mysql
categories:
 - Mysql
---


### mysql mysqldump 只导出表结构 不导出数据

``` bash
mysqldump --opt -d 数据库名 -u root -p > xxx.sql 
```
### 备份数据库 

``` bash
#mysqldump　数据库名　>数据库备份名 #mysqldump　-A　-u用户名　-p密码　数据库名>数据库备份名 
#mysqldump　-d　-A　--add-drop-table　-uroot　-p　>xxx.sql
```
 
### 1.导出结构不导出数据 

``` bash
mysqldump　--opt　-d　数据库名　-u　root　-p　>　xxx.sql　　 
```
### 2.导出数据不导出结构 

``` bash
mysqldump　-t　数据库名　-uroot　-p　>　xxx.sql
```

### 3.导出数据和表结构 

``` bash
mysqldump　数据库名　-uroot　-p　>　xxx.sql　 
```

### 4.导出特定表的结构 

``` bash
mysqldump　-uroot　-p　-B　数据库名　--table　表名　>　xxx.sql
```

### 导入数据： 

``` bash
#mysql　数据库名　<　文件名 
#source　/tmp/xxx.sql　
```