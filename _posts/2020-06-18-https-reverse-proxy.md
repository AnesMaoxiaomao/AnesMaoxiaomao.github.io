---
layout:     post   	  # 使用的布局（不需要改）
catalog: true
title:      HTTPS反向代理的设置	# 标题 
date:       2020-06-19 	
author:     麻辣小龙猫 	# 作者
categories:  工具					
tags:	#标签
    - 瞎搞博客日志
    - Nginx
    - VPS
    - HTTPS
    - SSL证书
    - Certbot
---

中午吃饭的时候一通操作，把博文的小绿锁又给加上去了，可喜可贺。<br>
全过程大概分为两个步骤：申请SSL证书，以及修改Nginx配置文件。

## 什么是HTTPS和SSL？
高中信息课好像有讲过HTTP协议。HTTPS就是加密的HTTP。具体怎么回事我也不是太懂，参考[这篇文章](https://www.runoob.com/w3cnote/https-ssl-intro.html)稍微扫一眼，对外行人来说应该也够用了。（万能借口：外行人！）

## Certbot申请SSL证书
[Certbot官网](https://certbot.eff.org/)里面讲得很清楚了。不赘述。

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200618142021917890.png)

选择Nginx和服务器的操作系统，按照提示操作即可。

#### 我遇到的坑
我的服务器操作系统是Debian 8，不知是因为系统太老旧还是什么原因，反正在安装过程中会跳出`UnicodeEncodeError: ‘ascii’ codec can’t encode character 89-99 `之类的错误。<br>
说明Nginx配置文件里有非ascii码表格里面的东西，在终端输入：

	grep -r -P "[^\x00-\x7F]" /etc/apache2
    
看结果发现——原来是我在配置文件里写的中文注释……
<br>

英文不好，写注释都要被软件歧视，哭哭。
<br>

统统删掉之后就能顺利安装了。

## 修改Nginx配置文件
在`/etc/nginx/conf.d`里新增`ssl.conf`

	ssl_certificate /etc/letsencrypt/live/www.xjmaoyaoyao.monster/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/www.xjmaoyaoyao.monster/privkey.pem;

把原来代理GitHub图床用的配置文件修改成：

    server {
        listen  443  https://www.xjmaoyaoyao.monster:1234;

        server_name  www.xjmaoyaoyao.monster;

        ssl_protocols TLSv1.2;
        ssl_ciphers HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;


        access_log   /home/site/access.log;

        location / {



            proxy_pass  https://raw.githubusercontent.com;

            proxy_set_header  Host          "raw.githubusercontent.com";

            proxy_set_header  Referer       raw.githubusercontent.com;

            proxy_set_header  X-Real-IP     $remote_addr;

            proxy_set_header  User-Agent    $http_user_agent;
        }
    }
    
 同理，本来代理WP的配置文件，修改成：
 
 
    server {
        listen  1234 ssl;

        server_name  www.xjmaoyaoyao.monster;

        ssl_protocols TLSv1.2;
        ssl_ciphers HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;


        access_log   /home/site/access.log;

        location / {
            proxy_pass  https://wordpress.com/;

            proxy_set_header  Host          "wordpress.com";

            proxy_set_header  Referer       wordpress.com;

            proxy_set_header  X-Real-IP     $remote_addr;

            proxy_set_header  User-Agent    $http_user_agent;
            
            proxy_set_header  Accept-Encoding  “”;

            sub_filter https://wordpress.com  https://www.xjmaoyaoyao.monster:1234;
            sub_filter_once off;
        }
    }

小绿锁就正常出现了。记得修改博文里图片的url。