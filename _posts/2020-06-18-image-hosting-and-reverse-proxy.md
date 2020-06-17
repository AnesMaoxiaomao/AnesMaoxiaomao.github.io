---
layout:     post   	  # 使用的布局（不需要改）
catalog: true
title:      本博客图床以及反向代理的设置	# 标题 
date:       2020-06-18 	
author:     麻辣小龙猫 	# 作者
categories:  工具					
tags:	#标签
    - 瞎搞博客日志
    - 图床
    - GitHub
    - Nginx
    - VPS
---

我博客图床有如下3个需求：免费、稳定不跑路、墙内可以看。朋友有推荐<https://imgbox.com/>、<https://www.privacypic.com/>、<https://postimages.org/>和国内的[路过图床](https://imgchr.com/)。但爱折腾如我，总归是想把图片数据牢牢抓在自己手中。于是就想到了万能的GitHub。

## GitHub仓库可以用作图床吗？
刚接触GitHub的时候，总觉得此站是高岭之花，是程序员的圣地，我等外行人不可轻易玷污。搞一个仓库，却不分享代码，仅仅用作图床，这种行为算是abuse吗？<br>

程序员朋友们早已想到了同样的（薅羊毛）好事，还在StackOverflow上提问，得到答复：“[It is allowed. ](https://stackoverflow.com/questions/23843721/hosting-file-mp3-and-images-on-github-is-it-allowed)”之后的but我暂且不去看它，反正就是普遍认为可以。法不禁止即可行。

## MDPIC-GitHub图床小工具
[MDPIC](https://github.com/skycity233/MDPIC)
亲测简单易用，推荐，感谢程序员。

---

然而，我没有想到， raw.githubusercontent.com ，是被墙掉的，不翻墙就完全看不到图。我希望我的博客可以在墙内也有很多的读者，所以放着不管是不行的。<br>

一种解决方法是逃避，回到文章开头，用那些工具。
另一种方法是折腾。反正这些年来各种加密的SOCKS代理、HTTP代理、透明代理之类的也都折腾过来了，再来个反向代理也没啥。

## 用Nginx设置反向代理

首先你要有一台VPS。<br>

其次要知道[什么是Nginx](https://www.yiibai.com/nginx)。<br>

然后，参考[这篇博文](https://blog.sometimesnaive.org/article/46.html)，部署反向代理如下：

	server {
        # 监听端口，其实可以随意
        listen  80;

        # 你的反代的域名
        server_name  www.xjmaoyaoyao.monster;

        # 记录访问日志
        access_log   /home/site/access.log;

        location / {
            # 指定我要反代的网站是 github
            proxy_pass  https://raw.githubusercontent.com;

            # 修改 request header Host 的值
            proxy_set_header  Host          "raw.githubusercontent.com";

            # 修改 request header Referer 的值
            proxy_set_header  Referer       raw.githubusercontent.com;

            # 修改 request header remote_addr 的值
            proxy_set_header  X-Real-IP     $remote_addr;

            # 修改 request header user_agent 的值
            proxy_set_header  User-Agent    $http_user_agent;
        }
    }

## 反向代理部署在其他端口
手上有VPS，端口80和443，常常被用于搞Trojan网页伪装，搞Mastodon，搞个人博客……这时也可以用Nginx布置反向代理，只不过要换个端口。如代理WP：

    server {
        # 监听端口，其实可以随意
        listen  1234;

        # 你的反代的域名
        server_name  www.xjmaoyaoyao.monster;

        # 记录访问日志
        access_log   /home/site/access.log;

        location / {
            # 指定我要反代的网站是 github
            proxy_pass  https://wordpress.com/;

            # 修改 request header Host 的值
            proxy_set_header  Host          "wordpress.com";

            # 修改 request header Referer 的值
            proxy_set_header  Referer       wordpress.com;

            # 修改 request header remote_addr 的值
            proxy_set_header  X-Real-IP     $remote_addr;

            # 修改 request header user_agent 的值
            proxy_set_header  User-Agent    $http_user_agent;
            proxy_set_header  Accept-Encoding  “”;

            sub_filter https://wordpress.com  http://www.xjmaoyaoyao.monster:1234;
            sub_filter_once off;
        }
    }

## 接下来的计划
研究HTTPS反向代理。带图博文左上角的小绿锁没有了，看上去挺糟心的。