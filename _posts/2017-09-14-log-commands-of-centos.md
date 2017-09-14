---
layout: post
title: CentOS 日志查看命令
date: 2017-09-14 20:19:00 +0800
categories:
- Tech
tags:
- CentOS

---


> "Everything is a file" describes one of the defining features of Unix.

### grep

``` bash
grep 'word' filename
grep 'word' file1 file2 file3
grep 'string1 string2'  filename
cat otherfile | grep 'something'
command | grep 'something'
command option1 | grep 'data'
grep --color 'data' fileName
```

more about `grep` in [How To Use grep Command In Linux / UNIX](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)



### How to View Logs Files?

``` bash
less /var/log/messages
more -f /var/log/messages
cat /var/log/messages
tail -f /var/log/messages
grep -i error /var/log/messages
```

quoted from [Linux Log Files Location And How Do I View Logs Files on Linux?](https://www.cyberciti.biz/faq/linux-log-files-location-and-how-do-i-view-logs-files/)


### Piping Work in Linux

`program1 arg arg | program2 arg arg`

**The vertical bar `|` is commonly referred to as a "pipe".** It is used to pipe one command into another. That is, it directs the output from the first command into the input for the second command. So your explanation is quite accurate.

more about `|` in

- [How Does Piping Work in Linux?](https://stackoverflow.com/questions/1072125/how-does-piping-work-in-linux)
- [Pipes: A Brief Introduction](http://www.linfo.org/pipes.html)
- [Pipeline (Unix)](https://en.wikipedia.org/wiki/Pipeline_%28Unix%29)