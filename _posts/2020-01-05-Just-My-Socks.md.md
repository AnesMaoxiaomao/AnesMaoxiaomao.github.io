---
layout:     post   				    # 使用的布局（不需要改）
title:     科学上网导论：献给学习欲旺盛的你 				# 标题 
subtitle:   翻墙基本概念知识  #副标题
date:       2020-01-05 				# 时间
author:     xjmaoyaoyao 						# 作者
header-img: https://pic2.zhimg.com/df33fb4b5d9a5ac2b14a18e0f6416692_1200x500.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 网络
    - CS
    - 翻墙（科学上网）
    - shadowsocks（ss）
    - VPN
    - 代理
---

阅读本文之前最好先读**网络分层、网络协议**的相关基础知识哦。当然没有这些基础，不求甚解地读也行啦，反正我自己也是这么不成系统地过来的。

***
代理服务器和VPN服务器都是常见的翻墙手法。他们看上去的原理都差不多，如图。
<br>
![](https://i.loli.net/2020/01/05/XjRGxvzLmDA3fZ6.png)
<br><br>


## 什么是代理服务器（proxy）？

> 代理服务器位于客户端和访问互联网之间，服务器接收客户端的请求，然后代替客户端向目标网站发出请求，所有的流量路由均来自代理服务器的IP地址，从而获取到一些不能直接获取的资源。

代理是你和你想访问办事对象的中介，中间商赚差价的那个中间商，在互联网的中介就是代理服务器。
<br><br>

![](https://wizardforcel.gitbooks.io/vbird-linux-server-3e/content/img/proxy_server_02.gif)

#### 常见的代理分为HTTP代理和SOCKET代理

> * HTTP代理是在HTTP协议层的代理服务，只能处理HTTP/HTTPS请求，主要满足用户Web浏览网页需求（**应用层**）。由于只处理HTTP请求，处理速度极快。它通常绑定在代理服务器的80、3128、8080等端口上。
> * SOCKET代理不解析网络流量，传递数据包而并不关心是何种应用协议（**传输层，TCP层**）。这使得SOCKET代理可以用于多种环境，支持FTP、SMTP、HTTP等。
> 	* SOCKS有SOCK4和SOCK5之分。其中SOCK4只支持TCP协议；SOCK5支持TCP和UDP协议，还支持身份验证、服务器端域名解释等。
<br>

总结：如果你纯粹用浏览器使用代理，HTTP 代理就够用了；如果你还需要让其它软件（比如MSN）也使用代理，那就得用SOCKS5代理。

例：下图是某无用大学图书馆代理的使用方法
![](http://www.library.fudan.edu.cn/_upload/article/images/f7/52/c319603b43f7b44d11f11e2e5f8d/9cc3f503-4f85-4b03-96b6-59adcb1cffa6.jpg)
<br>
可以很明显得看出是HTTP代理。通过这个代理可以访问图书馆资源，看到文献的全文。(虽然大部分人还是在用<https://sci-hub.se/>嘻嘻)
<br>
<br>

#### 普通代理的缺陷
如果你已经读过我写的网络协议科普，那你应当已经了解，HTTP协议是明文传输，在Request Message里面清清楚楚地写了请求的URL或IP地址。SOCKET协议的数据也是明文。这种超容易被GFW封锁的。
<br>
<br>那HTTPS就安全了？也不是啊，虽然通信加密了，可是跟代理服务器用CONNECT方法的时候也会明文提到要访问的远端地址。虽然不知道你出去干什么，但是去到哪里还是很明显的。
<br><br>


## 什么是VPN？

> 虚拟专用网（英语：Virtual Private Network，简称VPN），是一种常用于连接中、大型企业或团体与团体间的私人网络的通讯方法。虚拟私人网络的讯息透过公用的网络架构（例如：互联网）来传送内联网的网络讯息。它利用已加密的通道协议（Tunneling Protocol）来达到保密、发送端认证、消息准确性等私人消息安全效果

#### VPN的原理
> VPN是客户端使用相应的VPN协议先与VPN服务器进行通信，成功连接后就在操作系统内建立一个虚拟网卡，一般来说默认PC上所有网络通信都从这虚拟网卡上进出，经过VPN服务器中转之后再到达目的地。<br>简洁一点说：它是在**网络层，ip层**上工作的。<br>
> 常见情况下VPN协议会对数据流进行**强加密**处理，从而使得第三方无法知道数据内容，这样就实现了翻墙。翻墙时VPN服务器知道你干的所有事情（HTTP，对于HTTPS，它知道你去了哪）。

加密了！加密了的翻墙方法，在医院就是药代，在剧院就是黄牛，在妓院就是老鸨……
<br>
你也看到我都用的是什么词了，这些职业对加密技术和人品的要求都太高了，所以提醒大家最好**不要随便用不安全的VPN**。<br>

#### VPN的加密并非万无一失
VPN虽然加密了，但是特征很明显——虽然GFW不知道这些流量在干什么，但是鬼鬼祟祟的，背着一大袋子神神秘秘的东西天天跑来跑去，一看就不是好人！

***
**小结**：普通代理的方法特征不明显，然而没有加密。VPN有加密，但是特征太明显。

***
***
# 现在仍然好用的翻墙工具
可以分为两大类
1. 基于自购VPS或机场使用：**加密的代理服务器**
2. 不需要自购VPS或机场

### VPS是什么？
> VPS(Virtual Private Server 虚拟专用服务器)技术，将一部服务器分割成多个虚拟专享服务器的优质服务。每个VPS都可分配独立公网IP地址、独立操作系统、独立超大空间、独立内存、独立CPU资源、独立执行程序和独立系统配置等。

> 通俗的解释：VPS也相当于一台电脑，是服务商提供给你的一台电脑，但我们只有操作它的权利，并没有实物给你，在VPS上的操作就和我们操作家里的电脑是一样的。

我们之前提到了这么多次“服务器”，一般指的就是它了。
<br><br>

### 在哪里可以买到VPS？
[2019优质VPS服务商推荐](https://zhuanlan.zhihu.com/p/31856700)<br>
货比三家！

### 使用第一类翻墙软件的流程如下：
0. 购买VPS
1. 使用SSH工具，登录到VPS服务器
2. 使用命令行，安装服务器端软件并配置（大多有一键脚本）
3. （可选）安装bbr等加速软件
4. 在本地（Win/Mac/iOS/Android）安装客户端并配置。
5. 根据需要设置代理和PAC分流规则。
6. Enjoy! 
7. **……服务器的IP被墙了？请删掉你手上的服务器，并从0再次开始。** 吃饱了饭没事干，还可以给服务商发工单讨价还价。

### 如果嫌麻烦怎么办？
有商人懂得你的需求，购买了服务器并完成了服务器端的配置，你就只需要做上面的456步骤即可。速度不差，多条线路任君选择。<br>
黑话称他们的服务为**机场**。<br>
[更多VPS黑话](https://gist.github.com/biezhi/45fac901f02f7c867e46aecd41076d70#file-vps-md)
<br>

#### 机场安全吗？
> * 你的流量都从机场那边走了，虽然中间人能做的事情不多，但也不少，这是风险其一。
> * 第二，用户在机场注册、充值、使用时，会留下很多详细信息和日志，这个技术外的风险可就大了，最近爆出了某机场被抄，数千名用户的详细个人信息被警察一锅端回去了，后面会是什么场面简直不敢想象。从一个用户就可以牵出机场主，再由机场主就可以找到所有用户，这风险实在是太大太大了。

我是建议自己租VPS搭梯子啦，嫌麻烦的话直接买机场也不是不行，就是不够保险。
<br>好多机场在卖的时候都强调“不要翻墙做不爱国的事情”，我建议不要用这种。不是因为我有政治洁癖，而是因为机场老板这么战战兢兢的态度说明他尚未肉翻，随时可能被请去喝茶，到时候钞票可就找不回来了。<br>
<br>
私人机场主的人品可信吗？我个人根据自身经验，感觉这位推友说的就非常好：
![](https://img.pawoo.net/media_attachments/files/019/629/985/original/7320ef716cfe3c83.png)
<br>

#### 道理我都懂，可是真的好麻烦啊。
那就用国外大厂的机场吧。
推荐：<br>

Just My Socks 官网：<https://justmysocks.net >(已被墙)<br>

Just My Socks 备用官网：<https://justmysocks1.net>
<br>

JustMySocks 是搬瓦工官方提供的优质 Shadowsocks 服务，有 CN2 GIA 线路可选。被墙自动换 IP，无须担心 IP 被墙。<br>

## 开源的第一类工具有哪些？


### Shadowsocks（ss，影梭，小飞机）
> A secure socks5 proxy,
designed to protect your Internet traffic.


clowwindy大神被请去喝茶后，Shadowsocks项目已经从GitHub上被删除了。
[此处可下载源代码](https://github.com/shadowsocks/shadowsocks)<br>

[官网](https://shadowsocks.org/en/index.html)
<br><br>

### SSR
（暂不推荐，作者breakwall被请去喝茶后，项目已被删除，现在的混淆算法过时，不如用原版SS）
<br><br>

### Project V · V2Ray 
> Project V 是一个工具集合，它可以帮助你打造专属的基础通信网络。Project V 的核心工具称为`V2Ray`，其主要负责网络协议和功能的实现，与其它 Project V 通信。V2Ray 可以单独运行，也可以和其它工具配合，以提供简便的操作流程。


[项目地址](https://github.com/v2ray/v2ray-core)<br>

[官网（被墙）](https://www.v2ray.com/)<br>

[官方教程](https://guide.v2fly.org/)
<br><br>

### Trojan
> Trojan is an unidentifiable mechanism for bypassing GFW. This documentation introduces the trojan protocol, explains its underlying ideas, and provides a guide to it.

[项目地址](https://trojan-gfw.github.io/trojan/)<br>

[官网](https://trojan-gfw.github.io/trojan/)

<br><br>

## 第二类工具有哪些？

### Geph迷雾通
> Blast through censorship!<br>
Geph connects you with the censorship-free Internet,
even when nothing else works.


[项目地址](https://github.com/geph-official/)<br>

[官网](https://geph.io/)，好生嚣张高调！
<br><br>

我自己用的就是以上几款+Tor。Tor比较特殊，速度极慢，与其说是翻墙，不如说是为了安全。<br>
其他的我也不一一列举了。推荐编程随想的博客，大神不论是知识还是表达能力都比我强多了（笑）<br>

[编程随想：如何翻墙 (全方位入门扫盲)](https://program-think.blogspot.com/2009/05/how-to-break-through-gfw.html)<br>

[编程随想：2019年6月翻墙快报（兼谈用 I2P 突破封锁）](https://program-think.blogspot.com/2019/06/gfw-news.html)<br>

***
> 中华人民共和国的互联网创建于1987年。1987年9月20日，在北京ICA王运丰教授和西德卡尔斯鲁厄大学维尔纳·措恩教授的主导下，中华人民共和国大陆地区与外界互联网创建了首个连接。而中国第一封成功对外发出的电邮则是在1987年9月14日发出，内容为“Across the Great Wall, we can reach every corner in the world”（越过长城，走向世界每个角落）。