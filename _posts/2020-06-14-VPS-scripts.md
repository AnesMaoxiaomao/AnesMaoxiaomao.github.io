---
layout:     post   				    # 使用的布局（不需要改）
catalog: true
title:      服务器端一键安装翻墙脚本存档 				# 标题 
date:       2020-06-14 				# 时间
author:     麻辣小龙猫 						# 作者
categories:  工具					
tags:								#标签
    - 翻墙
    - shadowsocks
    - V2ray
    - Trojan
    - 一键脚本
    - BBR加速
---

## 服务器设置（推荐！）

#### 建有sudo权限的用户
例：

	sudo useradd -m -s /bin/bash trojanuser
	sudo passwd trojanuser
	sudo usermod -G sudo trojanuser
	su -l trojanuser

<br>

#### 建无sudo权限的用户
例：

	sudo groupadd certusers
	sudo useradd -r -M -G certusers trojan #无目录权限
	sudo useradd -r -m -G certusers acme #有目录权限
    
<!-- more -->
---


## 原版SS

CentOS 6+，Debian 7+，Ubuntu 12+<br>

	wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
	chmod +x shadowsocks.sh
	./shadowsocks.sh 2>&1 | tee shadowsocks.log

---

### SSR(不推荐)
	
	wget --no-check-certificate	 https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
	chmod +x shadowsocksR.sh
	./shadowsocksR.sh 2>&1 | tee shadowsocksR.log

---

## V2Ray

##### 时间校准
例：查看

	$ date -R
	Sun, 22 Jan 2017 10:10:36 -0500
    
<br>
例：更改

	$ sudo date --set="2017-01-22 16:16:23"
	Sun 22 Jan 16:16:23 GMT 2017

##### 一键安装
使用 Debian 8.x 以上或者 Ubuntu 16.04 以上的 Linux 系统

	$ wget https://install.direct/go.sh
	$ sudo bash go.sh

	$ sudo systemctl start v2ray
    
### V2Ray+WS+TLS配置

我是懒鬼，我选择Trojan自带的那个

---

## Trojan
Ubuntu 16.04 or Debian 9 及以上<br>
参考<https://www.johnrosen1.com/trojan/><br><br>

安装依赖：
	
	apt-get update && apt-get install whiptail sudo curl -y

一键脚本：

	sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/johnrosen1/trojan-gfw-script/master/vps.sh)"


---

## BBR

Trojan自带BBR加强，不用装

	wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
	chmod +x bbr.sh
	./bbr.sh

