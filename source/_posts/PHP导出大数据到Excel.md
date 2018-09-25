---
title: PHP导出大数据到Excel
date: 2017-06-29 16:03:21
tags:
 - PHP
categories:
 - PHP
---


数据量很大时（5万条以上），用 PHPExcel 导出 xls 将十分缓慢且占用很大内存，最终造成运行超时或内存不足。
可以通过设置 PHP 的运行时间和内存限制来阻止错误发生，但仍然不能确保导出完成。

``` bash
set_time_limit(0);
ini_set("memory_limit","512M");
```
### 一、 可生成多个链接分页进行导出

要彻底解决这个问题可以将数据分批导出成 CSV 格式的文件，这种格式简单导出快，并且也能用到 Excel 中。

我们用php提供的fputcsv来导出一百万的数据，原理就是打开一个标准输出流，然后把数据按一万条来分割，每一万条就刷新缓冲区。

``` bash
<?php
set_time_limit(0);
ini_set('memory_limit', '128M');
 
$fileName = date('YmdHis', time());
header('Content-Type: application/vnd.ms-execl');
header('Content-Disposition: attachment;filename="' . $fileName . '.csv"');
 
$begin = microtime(true);
 
//打开php标准输出流
//以写入追加的方式打开
$fp = fopen('php://output', 'a');
 
$db = new mysqli('127.0.0.1', 'root', '', 'test');
 
if($db->connect_error) {
    die('connect error');
}
 
//我们试着用fputcsv从数据库中导出1百万的数据
//我们每次取1万条数据，分100步来执行
//如果线上环境无法支持一次性读取1万条数据，可把$nums调小，$step相应增大。
$step = 100;
$nums = 10000;
 
//设置标题
$title = array('ID', '用户名', '用户年龄', '用户描述', '用户手机', '用户QQ', '用户邮箱', '用户地址');
foreach($title as $key => $item) {
    $title[$key] = iconv('UTF-8', 'GBK', $item);
}
//将标题写到标准输出中
fputcsv($fp, $title);
 
for($s = 1; $s <= $step; ++$s) {
    $start = ($s - 1) * $nums;
    $result = $db->query("SELECT * FROM tb_users ORDER BY id LIMIT {$start},{$nums}");
     
    if($result) {
        while($row = $result->fetch_assoc()) {
            foreach($row as $key => $item) {
                //这里必须转码，不然会乱码
                $row[$key] = iconv('UTF-8', 'GBK', $item);
            }
            fputcsv($fp, $row);
        }
        $result->free();
         
        //每1万条数据就刷新缓冲区
        ob_flush();
        flush();
    }
}
 
$end = microtime(true);
echo '用时：', $end - $begin;
```

生成一个文件数据过大，可以生成下载链接进行单个导出。
``` bash
  //表单
  <form action="<?php echo $this->createUrl('/offlinead/default/index');?>" id="myform" method="get" >
       <button type="button" class="btn btn-info" id="exportBtn">导出Excel</button>  
   </form>

   // 导出excel js
        $("#exportBtn").click(function () {
            var params = $("#myform").serialize();
             var url = "<?php echo $this->createUrl('/offlinead/default/index'); ?>";
             //获取查询数据的条数
            $.get(url+"?is_get_num=1&export=1&" + params, function(data) {
                var downDataList = "";
               
                if(data["rows"]) {
                    //rows是数据总条数，pageSize是一页多少条
                    var pageNum = Math.ceil(data["rows"] / data["pageSize"]);
                    for(var i = 1; i <= pageNum; ++i) {
                        downDataList += "<a href='"+url+"?export=1&" + params + "&page=" + i + "'>下载汇总结果" + i + "</a>&nbsp;&nbsp;";
                    }
                    $("#searchDataList").html(downDataList);
                } else {
                    commonjs.alert("没有数据");
                }
            }, "json");
            return false;
        });
```
### 二、 生成多个文件到一个目录，打包导出
JS代码
``` bash
       var pageNum = 0; //总页数
          // 导出excel
        $("#exportBtn").click(function () {
            var params = $("#myform").serialize();
            var url = "<?php echo $this->createUrl('/offlinead/default/index'); ?>";

             //获取查询数据的条数
            $.get(url+"?is_get_num=1&export=1&" + params, function(data) {
                if(data["rows"]) {
                    //rows是数据总条数，pageSize是一页多少条
                    pageNum = Math.ceil(data["rows"] / data["pageSize"]);
                    excel(1); //生成excel文件
                    console.log("正在生成第一页\n");
                 
                } else {
                    commonjs.alert("没有数据");
                }
            }, "json");
            return false;
        });
      //生成csv文件
        function excel(page) {
            //向后台发送处理数据
            $.ajax({
                type: "POST", 
                dataType: "text", 
                url: '<?php echo Yii::app()->createUrl("/offlinead/default/index"); ?>?' + $("#myform").serialize() + '&export=1&page='+page, //分页生成excel地址
                data: "",
                error: function () {
                    commonjs.alert("下载失败");
                },
                success: function (data) {
                    if (data == 1) {
                        var nowpage = page+1;
                        if (nowpage <= pageNum+1) {
                             excel(nowpage);
                             // console.log("正在生成第"+nowpage+"页\n");
                        }
                    } else if(data == 0) {
                        commonjs.alert("下载失败");
                    }else{
                        location.href = data; //跳转到生成的zip包进行下载
                    }
                }
            });
        }
```



PHP代码
``` bash
$list = $this->getOrder($this->page,$this->pageSize);//当前页查询到的数据

if ($this->page <= $pagenum) { //当前页小于总页数，生成csv
        $date = date("Ymd");
        $p =$this->page;//当前页数
        $filename = $this->_upload_path.$date."-".$p.'.csv';//$this->_upload_path生成文件路径
        $this->exportCSVCommand($filename,$list);//生成csv
        echo 1; //生成单页数据成功
    }else{ //全部生成完以后，对生成目录进行打包
        $zip=new ZipArchive();//php打包类,xxx.zip为压缩包文件名
        if($zip->open($this->_upload_path.'xxx.zip', ZipArchive::OVERWRITE)=== TRUE){
            $this->addFileToZip($this->_upload_path, $zip); //调用方法，对要打包的根目录进行操作，并将ZipArchive的对象传递给方法
            $zip->close(); //关闭处理的zip文件
            echo $this->_upload_path.'xxx.zip';//返回前端压缩包目录，进行下载
        }else{
            echo 0; //打包失败
        }
        
    }

//根据数组生成excel文件
    public  function exportArrayCommand($fileName, $array) {
        $fp = fopen($fileName, 'w');
        // 计数器
        $cnt = 0;
        // 每隔$limit行，刷新一下输出buffer
        $limit = 10000;
        // 逐行取出数据，不浪费内存
        foreach ($array as  $row) {
            $cnt ++;
            if ($limit == $cnt) { //刷新一下输出buffer，防止由于数据过多造成问题
                ob_flush();
                flush();
                $cnt = 0;
            }
            foreach ($row as $i => $v) {
                $row[$i] = iconv('utf-8', 'gb2312', $v);
            }
            fputcsv($fp, $row); 
        }
        fclose($fp);
    }


//将指定目录文件，加入到压缩文件对象
 public function addFileToZip($path,$zip){
        $handler=opendir($path); //打开当前文件夹由$path指定。
        while(($filename=readdir($handler))!==false){
            if($filename != "." && $filename != ".."){//文件夹文件名字为'.'和‘..’，不要对他们进行操作
                if(is_dir($path.DIRECTORY_SEPARATOR.$filename)){// 如果读取的某个对象是文件夹，则递归
                    addFileToZip($path."/".$filename, $zip);
                }else{ //将文件加入zip对象
                    $zip->addFile($path."/".$filename);
                }
            }
        }
        @closedir($path);
    }
```