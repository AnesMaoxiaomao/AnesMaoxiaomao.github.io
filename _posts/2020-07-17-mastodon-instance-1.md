---
layout:     post   	  # 使用的布局（不需要改）
catalog: true
title:      Mastodon 实例建站笔记（一）	# 标题 
date:       2020-07-17 	
author:     麻辣小龙猫 	# 作者
categories:  工具					
tags:	#标签
    - 瞎搞长毛象日志
    - Mastodon
    - VPS
    - 邮件服务
---

感谢[奈卜拉站长](https://nebula.moe/@neb)的[建站指南](https://i.nebula.moe/posts/2019-09-13-mastodon/)，感谢[屁屁老师](https://pullopen.xyz/@flyover)的[技术小白建站日志](https://pullopen.xyz/@flyover/104489942913001633)系列。
<br>
希望中文长毛象社区新实例遍地开花。希望我们都能自由地使用母语。

## 事前准备

### 一个域名
我自己这次用的域名是用来翻墙伪装的，冗长而奇怪。建议认真搞站选手买一个简介明了可爱的好域名。
域名选购网络上有很多教程，可参考[这篇博文](https://www.jianshu.com/p/a07ef52bf3a7)。

#### DNS解析改用cloudflare（可选）
优化加速什么我还没设置，凑合用。网络上教程也很多，不赘述。

### 邮件服务
我用的Zoho免费版，注册后验证域名——在DNS解析的地方加一个TXT记录即可。<br>
注意，如果是用其他账户（如google）注册Zoho的话，需要自己再设定一个密码，之后要用。

### VPS
VPS的选购网络上也有很多教程，不赘述。系统选 Ubuntu 18.04。

#### 服务器选哪家好？
* 有信用卡的选择的范围较多一些，没信用卡的乖乖搜哪些支持支付宝。
* 谷歌云可以免费用一段时间，前提是有信用卡。
* 注意商家是否能保证给中国能连的ip地址，可以发工单问。Vultr比较特殊，用多久算多少钱，可以自己不断换ip选一个最快的。
* 最好不要用阿里云等国内商家。
* 把服务器设在墙内的殆知阁又蠢又坏又鸡贼。

#### 选多大的呢？
一般来说，建一个十几到几十人的小型实例，大概需要一台10刀每月规格的机器。

#### 买完如何登服务器？
如非必要，不要用网页上的控制台。自己下个可以SSH的客户端就行。网上也有很多教程讲这些客户端的选择和使用方法，不赘述。

> SSH: Secure Shell is a cryptographic network protocol for operating network services securely over an unsecured network. Typical applications include remote command-line, login, and remote command execution, but any network service can be secured with SSH.

我自己用的是Bitvise SSH Client，它的优势在于不仅可以SSH，还可以SFTP，对于不熟悉命令行的朋友们非常方便。（是啦我就是草履虫）

![](https://naive.xjmaoyaoyao.monster/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200717114112864786.png)


<br><br>
另：密码登录很不安全，用SSH key。

#### SWAP
小鸡内存不够，必须要用SWAP，不然进程强制被杀欲哭无泪。
方法参见[屁屁老师博文](https://pullopenbluebox.wordpress.com/2020/06/16/cloudflare-nginx-swap-ssh/)，我自己弄了2G。

## 安装Mastodon
DO的直接用文章开头奈站长的教程。如果不幸（？）买了Vultr，那就按照[官方文档](https://docs.joinmastodon.org/admin/install/)。截至今天（2020-7-17），官方文档的版本是3.1.5。
<br>
过程大致分为：
*  **装系统：** curl、Node.js、Yarn、各种安装包（如nginx之类必要的东西）、装Ruby。我前几个月装Ruby的时候，官方文档好像有点问题，里面的依赖有些需要手动改；现在没啥问题，装起来丝般顺滑。
*  **正式装Mastodon：** 设置PostgreSQL、建新用户、然后`git clone`一番。

### 注意！建新用户这里有坑！
官方文档的命令是

	sudo -u postgres psql

这不行。
改成：
    
     su - postgres
     psql

其他都一样。

## 设置
	
    RAILS_ENV=production bundle exec rake mastodon:setup

此处会设置的东西存在`/home/mastodon/live/.env.production`里面，包括：

1. 域名，没啥好说的
2. 单用户模式？（我选no）
3. 用Docker？（我也选no）
4. PostgreSQL，这里其他都不用改，password和root的密码一样，复制一下贴进去。
5. Redis，这里一路回车，不设密码
6. SMTP邮件设置：
以Zoho为例：

        SMTP server: smtp.zoho.com
        SMTP port: 587
        SMTP username: 登录Zoho网站的用户名（自己注册用的邮箱账号）
        SMTP password: 登录Zoho网站的密码
        SMTP authentication: plain
        SMTP OpenSSL verify mode: none
        E-mail address to send e-mails "from": 在Zoho里面设置的那个邮箱，用户名@域名
        Send a test e-mail with this configuration right now? Yes
        Send test e-mail to: 自己随意一个外面的邮箱
        
完事之后一路回车即可安装，装完返回root账户。

### 建立管理员账户
不要输错。输错会出现神奇效果。
<br>
建好之后系统会给一串随机密码。之后登上去改掉就行。
<br><br>
输错也不要紧。
万能命令帮助你：

	RAILS_ENV=production bin/tootctl help

## 后续
继续按照官方文档，设置Nginx、申请SSL证书、启动 systemd 服务，没啥好说的。

## 精尽人亡，暂时不折腾别的了。