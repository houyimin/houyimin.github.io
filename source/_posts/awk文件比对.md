---
title: awk文件比对
date: 2018-06-25 17:30:34
tags:
 -  awk
categories:
 -  awk
---


## 文件以制表符\t分隔

### 1、将某几列作为主键key，对某列求和
``` bash
awk '{sum[$1,$2]+=$3}END{for(c in sum){print c,sum[c] }}' 1.txt > 3.txt
```
### 2、两个文件比较  while((getline < "1.txt")>0 //读到文件末尾
``` bash
cat 2|awk -F"\t" 'BEGIN{while((getline < "1")>0) hash[$1$2]=$3}{if(hash[$1$2]-$3>1 || hash[$1$2]-$3<-1) print $0"|"hash[$1$2]}'
```
### 3、提取某列输出重复数据项
``` bash
cat 1 |awk '{print $1}' |sort|uniq -c|awk '{if($1==2) print $0}'
```
### 4、在文件1不在文件2的数据项
``` bash
cat 1 2 1|sort |uniq -c|awk '{if($1==2) print $0}'
```

### 5、找出2个文件差异项
``` bash
cat 1.txt 2.txt |sort |uniq -u
```