---
title: 自动脚本安装showdoc
tags:showdoc
---
ShowDoc就是一个非常适合IT团队的在线文档分享工具，它可以加快团队之间沟通的效率。 [ShowDoc](https://www.showdoc.cc).

## 安装

#### 自动脚本脚本利用docker来安装运行环境，适用于linux服务器。

``` bash
#下载脚本并赋予权限
wget https://www.showdoc.cc/script/showdoc;chmod +x showdoc;
#默认安装中文版。如果想安装英文版，请加上en参数，如 ./showdoc en
./showdoc
```


#### 其他命令

``` bash
#执行上面命令便会自动安装完成。下面附上脚本其他命令，以便管理showdoc时可以用得上。
#停止
./showdoc stop 
#重启
./showdoc restart
#升级showdoc到最新版
./showdoc update
#卸载showdoc
./showdoc uninstall
```

## 使用说明

安装好后，showdoc的数据都会存放在 /showdoc_data/html 目录下。./showdoc 脚本可放置在任何目录，方便以后使用。也可以重新从官方地址下载。

你可以打开 http://xxx.com:4999 来访问showdoc (xxx.com为你的服务器域名或者IP)。账户密码是showdoc/123456

## 从手动方式升级到自动脚本方式

1. 把原来showdoc目录的Sqlite/showdoc.db.php覆盖/showdoc_data/html/Sqlite/showdoc.db.php ，Public/Uploads覆盖 /showdoc_data/html/Public/Uploads
2. 执行命令

``` bash
chmod 777 -R /showdoc_data/html
 ./showdoc update
```
## 自动安装shell脚本

``` bash
#!/bin/bash
if [[  -n "$1" ]] ; then 
	action=$1
else
	action='install'
fi
if [ "$action" == "start" ] ;then
	sudo -s  service docker start
	sudo -s docker start showdoc
	exit 1 
fi
if [ "$action" == "restart" ] ;then
	sudo -s docker restart showdoc 
	exit 1
fi
if [ "$action" == "stop" ] ;then
	sudo -s docker stop showdoc 
	exit 1
fi
if [ "$action" == "update" ] ;then
	DATE=$(date +%Y%m%d_%H%M%S_%N) 
	if [ ! -d "/showdoc_data/html" ]; then 
		echo "/showdoc_data/html 目录不存在"
		exit 1 ;
	fi
	sudo -s docker stop showdoc 
	rm -f master.tar.gz
	wget https://github.com/star7th/showdoc/archive/master.tar.gz
	if [ ! -f "master.tar.gz" ]; then 
		docker start showdoc
		echo "文件下载失败" 
		exit 1 
	fi
	sudo -s mv /showdoc_data/html /showdoc_data/html_bak_${DATE}
	tar -zxvf master.tar.gz -C /showdoc_data/ 
	sudo -s mv /showdoc_data/showdoc-master /showdoc_data/html  ##// */
	if  [ ! -d "/showdoc_data/html" ]; then 
		echo "/showdoc_data/html 目录不存在"
		exit 1 ;
	fi
	sudo -s chmod 777 -R /showdoc_data/html
	sudo -s docker start showdoc
	sleep 10
	curl http://localhost:4999/install/non_interactive.php?lang=zh
	\cp  -f  /showdoc_data/html_bak_${DATE}/Sqlite/showdoc.db.php /showdoc_data/html/Sqlite/showdoc.db.php
	\cp -r -f /showdoc_data/html_bak_${DATE}/Public/Uploads /showdoc_data/html/Public/Uploads
	sudo -s curl http://localhost:4999?s=/home/update/db
	rm -f master.tar.gz

	exit 1
fi
if [ "$action" == "uninstall" ] ;then
	read -r -p "即将卸载showdoc，你是否确认删除showdoc所有数据? [Y/n] " input
	case $input in
	    [yY][eE][sS]|[yY])
				echo "正在卸载..."
				sudo -s docker stop showdoc
				sudo -s docker rm showdoc
				sudo -s docker rmi registry.docker-cn.com/star7th/showdoc
				sudo -s rm -rf /showdoc_data

			;;

	    [nN][oO]|[nN])
	       		;;
	    *)
		exit 1
		;;
	esac
	exit 1
fi
if ! [ -x "$(command -v docker)" ]; then
  echo '检测到Docker尚未安装。正在试图从网络安装...所需时间与你的网络环境有关'
  sudo -s curl -sSL https://get.daocloud.io/docker | sh
  sudo -s  chkconfig docker on 
fi
if ! [ -x "$(command -v docker)" ]; then
  echo '检测到Docker尚未安装。正在试图从网络安装...所需时间与你的网络环境有关'
  sudo -s curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
  sudo -s  chkconfig docker on 
fi
if ! [ -x "$(command -v docker)" ]; then
  echo 'Docker自动安装失败,建议你手动安装好docker环境后再启动本脚本' 
  exit 1 
fi

if  [  "$(docker images  |grep showdoc)" ]; then
  echo "你已经安装过showdoc镜像"
  echo "如果你想更新showdoc，请执行  ./showdoc update "
  echo "如果你想重启showdoc，请执行  ./showdoc restart "
  echo "如果你想卸载showdoc，请执行  ./showdoc uninstall "
  exit 1 
fi
sudo -s  service docker start
echo '正在拉取showdoc镜像，请稍后...所需时间与你的网络环境有关'
sudo -s docker pull registry.docker-cn.com/star7th/showdoc
sudo -s mkdir /showdoc_data
if  [ ! -d "/showdoc_data" ]; then 
	echo "/showdoc_data 目录不存在，请确保有创建权限"
	exit 1 ;
fi
sudo -s mkdir /showdoc_data/html
sudo -s chmod 777 -R /showdoc_data
sudo -s docker run -d --name showdoc -p 4999:80 -v /showdoc_data/html:/var/www/html/ registry.docker-cn.com/star7th/showdoc
sleep 10
sudo -s docker exec showdoc \cp -fr /showdoc_data/html/ /var/www/
sudo -s chmod 777 -R /showdoc_data
if [ "$action" == "en" ] ;then
 	sudo -s curl http://localhost:4999/install/non_interactive.php?lang=en
else
	sudo -s curl http://localhost:4999/install/non_interactive.php?lang=zh
fi
sudo -s wget http://localhost:4999/install/install.lock
if [  -f "install.lock" ]; then 
  rm -rf install.lock 
  echo -e "\n \033[32m 安装成功，访问地址：http://localhost:4999 (你也可以用局域网或者公网IP/域名访问)  \033[0m \n"
  echo -e " \033[32m 账户密码是showdoc/123456，登录后你便可以看到右上方的管理后台入口。建议登录后修改密码。   \033[0m \n"
  echo -e " \033[32m 对showdoc的问题或建议请至https://github.com/star7th/showdoc 提issue   \033[0m \n"

fi
```


