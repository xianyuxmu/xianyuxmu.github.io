---
layout: post
title: 开发常用命令
date: 2018-07-10 20:09:00 +0800
categories:
- Tech
tags:
- 开发

---


### Linux Vi/Vim

``` bash
# 打开文件
vi filename :打开或新建文件，并将光标置于第一行首 
vi + filename ：打开文件，并将光标置于最后一行首 
vi +n filename ：打开文件，并将光标置于第n行首

# 插入文本
i ：在光标前 
I ：在当前行首 
a：光标后 
A：在当前行尾 

# 退出保存
:wq 保存文件，退出
:q 退出。防止没有保存就退出。
:q! 退出。无论保存与否，都退出。
:wq! 强制保存

# 光标移动
h    左一个字符
j    下一行
k    上一行
l    右一个字符
w, W    前一个单词 (W 忽略标点)
b, B    后一个单词 (B 忽略标点)
$    到行尾
^    到行首第一个非空字符
0    行首
G    到缓冲首
nG    到第 n 行

# 删除搜索
dd       删除一行
/pattern：从光标开始处向文件尾搜索pattern 

```