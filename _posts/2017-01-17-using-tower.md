---
layout: post
title: '使用 Tower 提高 Git 效率'
date: 2017-01-17 18:00:48 +0800
categories:
- Work
tags:
- Productively
- Git

---

## 选择 Git

[什么是版本控制？](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/what-is-version-control#start)

[为什么要使用版本控制系统？](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/why-use-version-control#start)

[命令行界面还是图形界面？](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/getting-ready#start)

## 通过终端使用 Git

在技术项目开发中，我们一般会用 Git 或者 SVN 来进行版本控制，我们的团队中，采用了 Git。

[为什么选择 Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/why-git)

1. Git 运行速度快；
2. 可以离线工作；
3. 可以撤销错误操作；
4. 可靠性高，本地克隆可以备份整个项目，Git 操作几乎只做提交；



通过终端(Terminal)来操作 Git 是很常见的。在日常开发中，每天会进行很多的操作：checkout、merge、rebase、commit、discard 等等。

终端虽好，可不要贪杯哦。终端还是有以下的一些缺点的：

1. 不直观。分支查看需要命令、commit log 查看需要命令、当前变更状态需要命令。
2. 操作繁琐。在一次 commit 前，一般要先选择待提交的文件，使用 `git add .` 可以添加所有，但是要是遇到大量的文件变更要分次提交时，就是一件很费时的事；rebase 远程仓库代码时，需要先看一下远程仓库的分支，然后再执行 rebase 代码；在 rebase、merge 代码时，可能有冲突需要解决，你就需要不断地合并、不断地 continue。
3. .... 

当然通过终端使用的 Git 也是有好处的，就不多说了！

其它不加赘述，这两个点已经很要命了。

[Tower](https://www.git-tower.com/) 提供了一套 GUI 的方式来操作 Git，在平时使用的时候更加便利。

## Tower 文章导航

- [Tower Help](https://www.git-tower.com/help/mac)
- 

## Tower 常用的快捷键

[Tower - Keyboard Shortcuts](https://www.git-tower.com/help/mac/faq-and-tips/keyboard-shortcuts)

### 各种工作视图的切换

1. `⌘ + CTRL + R` 快速回到仓库列表
2. `⌘ + 1` 查看 Working Copy 目录
3. `⌘ + 2` 查看 History
4. `⌘ + 3` 查看 Stashes
5. `⌘ + 0` 回到 HEAD 分支
6. `⌘ + N` 打开新窗口
7. `⌘+Return` 提交 Commit
8. `⌘ + ⇧ + O` 快速打开 Repositories
9. `⌘ + O` 打开文件
10. `⌘ + T` 新建 Tab
11. `⌘ + B` 新建分支

### 工作区和本地修改

1. `⌘ + ⇧ + C` 激活提交对话框 commit dialog
2. `SPACEBAR`(空格键) 选中或取消选中(Stage/unstage) 修改的文件
3. `⌘ + ⇧ + A` 选中(Stage)所有当前修改的内容
4. `⌘ + ⇧ + ⌥ + A` Unstage all current changes
5. `⌘ + ⇧ + S` Save to Stash
6. `⌘ + ⇧ + ⌥ + S` Apply Stash
7. `⌘ + ⇧ + BACKSPACE` Discard local changes in selected file
8. `⌘ + CTRL + I` Show / hide ignored files

### 远程交互和提交历史

1. `⌘ + ⇧ + F` Fetch
2. `⌘ + ⇧ + P` Pull
3. `⌘ + ⇧ + U` Push HEAD
4. `⌘ + CTRL + G` Show / hide commit tree graph
5. `⌘ + C` Copy SHA-1 hashes of selected commits to clipboard
6. `⌘ + CTRL + →` Expand all diffs in changeset
7. `⌘ + CTRL + ←` Collapse all diffs in changeset

### Merging & Rebasing

1. `⌘ + ⇧ + M` Merge
2. `⌘ + ⇧ + R` Rebase

### Creating Branches & Tags

1. `⌘ + B` Create new branch
2. `⌘ + ⇧ + T` Create new tag

----

## Tower Tips & Tricks

### 解决冲突／Solving Merge Conflicts

即使 Git 非常擅长于解决冲突，但是在使用 Git 过程中比，也经常需要人工区解决冲突。常见的冲突是**同一行**被不同的分支修改了，你就需要**告诉 Git 如何解决问题**。Tower 在出现冲突的时候，会给出提示，处理完冲突后点击 `Continue`。

- "MY VERSION" 指的是
- "THEIR VERSION" 指的是

冲突情况：

1. 选择其中一个文件解决冲突。点击选择 `"MY VERSION"` 或者 `"THEIR VERSION"`，然后点击 `Resolve Using <My / Their> Version` 按钮即可。
2. 合并两个文件来解决冲突。需要通过 `Open <Merge Tool>` 来解决冲突。


### Services 功能

Services 功能帮助我们集成多个版本控制服务平台(GitHub、BitBucket等)，使用快捷键 `⌘ + CTRL + S` 打开 Services，添加对应平台的账号。


### [Drag & Drop](https://www.git-tower.com/help/mac/faq-and-tips/tips-and-tricks#faq-drag-drop)

1. 拖动分支到另外一个分支可以执行 `MERGE` 或者 `PUSH` 操作
2. 拖动选中的提交到本地 `HEAD` 分支可以执行 `CHERRY-PICK`
3. 拖动选中的提交到左侧的 `Tags` 可以执行 `NEW TAG` 来新建 tag
4. 拖动 `Stashes` 中的部分文件到 `Branches` 可以部分应用(APPLY) Stashes


### 克隆仓库默认路径

在设置中设置 `Default directory for cloned repositories`后，新克隆的仓库就会存储在这个路径下。

### Quick Open

执行 `⌘ + ⇧ + O` 可以快速打开仓库(Repository)。

### Amend

按住 `⌥` 按键时，`Commit` 按钮会变成 `Amend`。

### 仓库排序／Sorting Bookmarks

通过 `⌘ + CTRL + R` 快速回到仓库列表，然后点击右键，选择 `Sort Parent Folder by Name` 以对 `Repositories` 进行排序。

### 复制提交信息／Copy Commit Info

通过快捷键 `⌘ + 2` 跳转到 `History`，在记录中点击右键调出菜单，然后选择 `Copy Commit Info to Clipboard`。

### Quick Actions for Merge, Rebase, Pull, Push, Fetch

在执行 Merge、Rebase、Pull、Push、Fetch 时按住 `⌥` 按键，可以快速提交而不用跳出二次确认框。

### 快速创建分支

执行 `⌘ + ⇧ + B` 打开 `Check Out Revision` 对话框。

### 克隆／Clone

执行 `⌘ + ⌃ + C` 调出 `Clone` 对话框，在对话框中可以直接查找 `GitHub` 上的项目。

### Cherry Pick

The "cherry-pick" feature allows you to integrate individual commits into your current HEAD branch - instead of having to integrate all of a branch's commits (like with merge and rebase).

Tower allows you to perform a cherry-pick in two ways. After selecting the commits in any of Tower's commit listings you can either...

- right-click and choose the Cherry-Pick option from the contextual menu or...
- drag the commit(s) and drop them onto the Working Copy item in Tower's sidebar.

### 合并提交／Squash N last commits

命令：`git rebase --interactive --autosquash HEAD~N`

> As a golden rule you should never rebase commits that have already been published on a remote repository (pushed).

不要对被推到远程仓库的 `commits` 执行合并提交操作 `git rebase -i HEAD~~`

### 仓库设置／Repository Settings

通过 `Repository Settings` 可以设置提交者身份、仓库描述、

----

## Tower 几个注意点

1. 远程数据是一个快照（Snapshot）。Git 会在你的本地仓库中保存远程数据的信息，但是它并不是 “实时地” 连接到你的远程仓库上的。需要通过 `Fetch` 来更新远程仓库快照。
2. 跟踪分支。一般来说，分支之间并无任何关系，然而我们可以定义一个本地分支去 “跟踪 （track）” 一个远程分支。Tower 会自动提示本地分支与对应的远程分支的差异，看是 **领先（ahead）** 还是 **落后（behind）**。


## Tower 使用流程

[A Basic Workflow](https://www.git-tower.com/help/mac/first-steps/basic-workflow)

End by Robin on February 07, 2017

2ed work day of 2017