---
layout: post
title: "linux上安装nodejs+npm+pm2环境"
date: 2018-03-11
description: "linux上安装nodejs+npm+pm2环境"
tag: 服务器部署
---

### 一.安装nodejs+npm环境
+ 添加一个nodejs用户（或切换至运维用户，注意需要非root权限）

```
adduser nodejs //这一步看具体情况，是否需要！！
passwd nodejs  //这一步看具体情况，是否需要！！
```

+ 下载以及安装 [node.js](https://nodejs.org/en/download/)
如果下载source版本，需要通过configure/make/make install进行安装，不过可能会由于依赖包问题需要升级c++
建议直接下载二进制文件包，解压可以直接使用

+ 这里我们先把下载的node-vx.x.x-linux-x64.tar.xz 包放在linux 用户根目录的Downloads目录下。

	> ps: mac本地下载tar包上传linux服务器的命令

	> 语法：scp /Users/tangsicheng/Downloads/node-xxxx-linux-x64.tar.xz  <服务器用户名>@<服务器地址>:<安装目录绝对路径>

	```
	scp /Users/tangsicheng/Downloads/node-xxxx-linux-x64.tar.xz  tsc@10.1.1.1:/home/tsc/Downloads
	```

	然后依次执行下列命令解压

	```
	su - nodejs //切换账号，看情况执行！
	cd ~/Downloads/
	xz -d node-xxxx-linux-x64.tar.xz
	tar -xvf node-xxxx-linux-x64.tar
	```

	之后我选择把解压后的文件重新命名为nodejs，并且放到用户根目录下，这里可以自由选择到底放在哪个目录下。

	```
	mv node-v8.9.3-linux-x64 /home/<userName>/nodejs
	cd ~
	ls
	```
	现在应该就可以在用户根目录下看到nodejs的文件夹了。然后我们开始做软连接。


	```
	sudo ln -s /home/<userName>/nodejs/bin/node /usr/local/bin/node
	sudo ln -s /home/<userName>/nodejs/bin/npm /usr/local/bin/npm
	```


+ 验证是否已经成功安装node，如果出现下面的情况就OK了

	```
	node -v
	v6.11.0
	npm -v
	3.10.10
	```


+ 安装npm的中国淘宝镜像

	```
	npm get registry //获取目前的镜像地址

	npm config set registry http://registry.npm.taobao.org/  //设置淘宝镜像

	npm config set registry https://registry.npmjs.org/   //还原成最新的镜像

	```


### 安装pm2 进程守护

* 全局引入pm2依赖

	```
	npm install pm2 -g
	```

* 软连接
	
	```
	sudo ln -s /home/axxt/nodejs/bin/pm2  /usr/local/bin/pm2
	```

* 验证

	```
	 pm2 -v
	2.10.1
	```
	看到出现版本号了，说明就正确了。




