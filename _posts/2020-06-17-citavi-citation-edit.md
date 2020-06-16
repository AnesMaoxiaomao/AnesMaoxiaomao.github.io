---
layout:     post   				    # 使用的布局（不需要改）
catalog: true
title:      Citavi 引用格式修改
subtitle:  以国标 GB/T 7714-2015 为例   # 标题 
date:       2020-06-17				# 时间
author:     麻辣小龙猫 						# 作者
categories:  工具					
tags:								#标签
    - Citavi
    - 文献管理软件
    - 国标 GB/T 7714-2015
    - 论文写作
    - 参考文献格式
    - 医生/医学生为什么要用 GitHub
---

正常的文献管理软件都会支持特定格式参考文献的导出。然而，软件内置的格式，虽然名称是那个名称，但形式却并不符合盲审老师的要求。以国标 GB/T 7714-2015 为例：

<!-- more -->

## 国标 GB/T 7714-2015

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200616235918053273.png)

咱国高校毕业论文要求里一般都会附上这么一个纸质扫描版的pdf供学生们检阅，其中给出了一系列的规范在此不赘述。<br>

## 真实国标与软件导出国标格式的区别

一般来说，最常引用的是期刊文章（[J]），其中，英文文献的大小写很容易出错。<br>

真实国标文件给出的示例：<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617000726071796.png)

<br>
Citavi软件里，网友提供了国标 citation style，生成的参考文献长这样：<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617001728289287.png)

<br>
不同点在于：<br>

1. 作者姓名大小写。（作者姓氏所有字母大写，名用首字母缩写）
2. 文章标题大小写。（各杂志导出情况比较混乱，不一定是“首字母大写，其他均小写”。）
3. 杂志名称大小写。（“按英文习惯规定”，实际上是正常词语首字母大写，虚词全部小写。）

毕业生，盲审deadline当头，预审意见有12345678条要逐一修改，好心的师兄师姐催着问有没有改好快给ta看看……在这当头，怎么可能对着一百多条参考文献肉眼手工修改呢？

## 批量修改引用样式
![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617002825626288.png)
<br>
Citavi支持用户自己修改引用样式。<br>
网络上有Citavi中文样式修改教程，我这里就不重复造轮子了。
<https://softhead.cowtransfer.com/s/5d92cfe216e242>
此教程由 Drei T. Stein 贡献，QQ：707420060
<br>
按照教程可以解决前两个问题，但“英文习惯”这种东西，Citavi并没有内置。

## 过滤器的使用

以“杂志名称大小写”为例：<br>1. 如前所述 Ctrl + Shift + F11，选择要修改的样式，进入编辑窗口

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617004150682095.png)
<br>2. 单击 Journal Artical，双击 .Periodical<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617005445580525.png)
<br>3. 进入新窗口后，点击空白部位，取消对 .Periodical 的选择，并点选 Filter过滤器 的选框<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617005700750780.png)
<br>4. Add filter<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617005902669235.png)
<br>5. 学C#<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617010420528065.png)
<br>

开个玩笑，学C#自然是很好，但**外行人就不要想着自己造轮子了！**

---

## 上GitHub找现成的过滤器代码
Citavi官方有GitHub账号<https://github.com/Citavi>，他们搞了四个仓库：<br>

![](https://raw.githubusercontent.com/malaxiaolongmao/MLXLMblogPictures/master/images/image_20200617011037922548.png)
<br>
去那里找我们需要的代码，不言自明~<br>
对于本文之前提的要求：
>杂志名称大小写。（“按英文习惯规定”，实际上是正常词语首字母大写，虚词全部小写。）

Citavi 官方给出了[完美的答案](https://github.com/Citavi/C6-Citation-Style-Scripts/blob/master/Components/COT%20Other/COT007%20Capitalize%20first%20letter%20of%20simple%20text%20field%20elements/COT007_Capitalize_first_letter_of_simple_text_field_elements.cs>)。
<br>

之后怎么做就不用我一步步讲了吧？

## 其他软件？
[EndNote](https://github.com/topics/endnote)、[Zotero](https://github.com/zotero)、[Mendeley](https://github.com/Mendeley)……我全都没深入用过，不了解。有需求自己上网搜啦，原理都一样的，反正就是GitHub啦。<br>

Hashtag: #医生/医学生为什么要用 GitHub