---
layout: post
title: "Space Phone Book – 使用C实现SQL"
date: 2016-01-22 23:50:14 +0800
categories:
- Works
- Tech
tags:
- 个人作品
- 我的大学
- C语言
---


**Space Phone Book** － 这是一个使用C实现的类似“SQL”的Win32控制台“电话本”应用。

概览：

1. 用C实现SQL类似功能。我自己相处了一个简单的架构，就是把命令识别模块独立出来，所有输入的命令将又这个命令识别模块统一进行分词、识别、分发操作，最终反馈结果。

2. 用户每个命令都会得到响应和建议。这个程序不仅实现了命令识别，我们还实现对错误命令直接给用户说明是哪里出了错，做到Response to ANY，甚至，我们还对对错误命令给出纠正的建议。

3. 类似Spot Light的超级搜索引擎(Space Search)。我们实现了模糊查询，你可以输入姓名的首字母即可找出你要的结果，同时，当你输入类似“name vinci”，我们会根据分词“name”先去查找这个名字。

4. 人性化的用户体验。控制台？体验？是的，我们设计这个产品的出发点就是“任何人都能轻易上手”。是的，我们确实考虑到了这些，也做到了。

5. “零”Bug。是的，这你也没看错。我们的程序运行非常的稳定，做到这些是因为我们考虑到了所有的情况。更重要的是，得益于我们产品强大的命令识别模块，我们运用合理的架构把错误处理分隔开来。

 

Space Phone Book项目是我在2013年7月份，也就是大一的第三个学期(在厦大我们都叫这个学期为小学期，只有5周的时间，类似国外的Summer Course)。当时，小学期开设的是开放性的实践课程，因为我们大一学了C和C++两门语言，所以我们的实践课程就采用C系的语言作为课程统用语言。

我和其他三个好朋友一同组建了“Space”(中文名:空格键)小组，我们的作品都采用Space进行命名，Space寓意为：空间、宇宙、空格、空格键。

![image](/uploads/space-phone-book/space-phone-book-1.jpg)

我们当时的角色都是一样的，都是从零开始探索一个课题。

<!-- more -->

第一周，实践的课题是把输入的算数表达式转化成逆波兰表达式，并计算出成果。

第二三周，我们实现了今天要讲的重磅产品－Space Phone Book

![image](/uploads/space-phone-book/space-phone-book-2.png)

<center>我们引入了“图形界面”</center>

 

我们使用C语言在控制台下面实现了一个极类似“SQL”操作形式的电话本。

![image](/uploads/space-phone-book/space-phone-book-3.png)

<center>主界面</center>

 

insert、delete、update、select、group…结构化查询语句一应具全。

好吧，我还是直接说项目得意之处吧…

1、表格绘画模块


![image](/uploads/space-phone-book/space-phone-book-4.png)

<center>按地点进行排序A、B、C…</center>


![image](/uploads/space-phone-book/space-phone-book-5.png)

<center>select 一行，没问题</center>

![image](/uploads/space-phone-book/space-phone-book-6.png)


<center>select 两行，没问题</center>

![image](/uploads/space-phone-book/space-phone-book-7.png)

好吧，这个功能跟SQL是一样的，说真的，用C写真不好写

2、Space Search(万能搜索，跟Mac上的Spot Light类似)

![image](/uploads/space-phone-book/space-phone-book-8.png)

<center>模糊查询“m i”显示结果为Mike</center>

![image](/uploads/space-phone-book/space-phone-book-9.png)

<center>分词，从“I love you，kate”中分出“kate”关键词</center>

![image](/uploads/space-phone-book/space-phone-book-10.png)

<center>输入“BankSchool”自动查找Office为“bank”“School”的结果</center>

3、命令识别模块

![image](/uploads/space-phone-book/space-phone-book-11.png)

<center>任意输入，不会挂，还会提醒你哪个位置出错</center>

![image](/uploads/space-phone-book/space-phone-book-12.png)

<center>对错误的命令进行提示</center>

总结：

这是一个很小的项目，但当时，我们团队倾尽自己的时间、知识去创造出我们能做到的最好的水准。我们获得了那个小学期的小组全班第二名(80人的班)，我个人获得“最佳代码”荣誉！

———————————-

我们当时做pre的的课件：http://pan.baidu.com/s/1nt9tw7n

项目GitHub地址：https://github.com/xianyuxmu/Space-Phone-Book