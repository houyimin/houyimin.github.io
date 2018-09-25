---
title: Python导出CSV
date: 2018-07-17 10:50:38
tags:
 - Python
categories:
 - Python
---


``` bash
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import pymysql
import io
import sys
import csv
import time


sys.stdout = io.TextIOWrapper(sys.stdout.buffer,encoding='gb18030')         #改变标准输出的默认编码
 
sql ="SELECT hospital_id, type FROM `tb_hospital`"

start = time.time()

db = pymysql.connect(host='127.0.0.1',user='root', password='123456', port=3306, db='db_kr',charset='utf8')
cursor = db.cursor()

cursor.execute(sql)
results = cursor.fetchall()
print('Count:', cursor.rowcount)
i=0
with open('hospital_type.csv', 'a+', encoding='utf-8',newline='') as csvfile:
	writer = csv.writer(csvfile)
	writer.writerow([ 'hospital_id', '类型'])
	for row in results:
		i = i + 1
		print("第",i)
		writer.writerow(row)

end = time.time()
print('Cost time:',end-start)
```