---
title: 使用Python将HTML转成PDF
date: 2018-10-21 16:37:40
tags:
 - Python
categories:
 - Python
---

> 主要使用的是wkhtmltopdf的Python封装——pdfkit 


### 1、Install python-pdfkit

``` bash
pip install pdfkit
```

### 2、Install wkhtmltopdf

Linux
在[wkhtmltopdf](https://wkhtmltopdf.org/downloads.html)下载对应的文件, 解压文件到环境变量所在目录, 例如 /opt 或者 /usr/local/bin 然后在此执行.

Debian / Ubuntu
``` bash
apt-get install wkhtmltopdf
```

Windows
在[wkhtmltopdf](https://wkhtmltopdf.org/downloads.html)下载对应安装包，安装完成后将可执行文件目录加入环境变量.



### 使用

``` bash
	import pdfkit

    pdfkit.from_url('http://google.com', 'out.pdf')
    pdfkit.from_file('test.html', 'out.pdf')
    pdfkit.from_string('Hello!', 'out.pdf')
```

传递url或者文件列表
``` bash
	pdfkit.from_url(['google.com', 'yandex.ru', 'engadget.com'], 'out.pdf')
    pdfkit.from_file(['file1.html', 'file2.html'], 'out.pdf')
```

传递一个打开的文件
``` bash
	with open('file.html') as f:
        pdfkit.from_file(f, 'out.pdf')
```

如果你想对生成的PDF作进一步处理， 你可以将其读取到一个变量中:
设置输出文件为False，将结果赋给一个变量
``` bash
    pdf = pdfkit.from_url('http://google.com', False)
```


> 参考链接
	[使用Python将HTML转成PDF](http://www.cnblogs.com/taceywong/p/5643978.html)
 	wkhtmltopdf安装教程 [Installing-wkhtmltopdf](https://github.com/JazzCore/python-pdfkit/wiki/Installing-wkhtmltopdf)
