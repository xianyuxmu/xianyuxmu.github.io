---
layout: post
title: "Git Study Notes"
date: 2016-07-07 16:45:00 +0800
categories:
- Technology
tags:
- Git
---

### *Git Study Notes*

#### ***Basic Usage:***

Git 基本用法以及命令详解：[图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html#conventions)

Git-Submodule: [SubModule - Git 中文文档](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

Git-Rebase: [Rebase - Git 中文文档](http://gitbook.liuhui998.com/4_2.html)


分支管理策略：[Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)

Git远程操作详解:[Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

Git - stash用法:[Git - stash用法](https://segmentfault.com/a/1190000002554160)

---


#### 个人 Git 模拟测试：

如图1：

![Git log 图](/uploads/tech/git_study_notes/git_study_notes_1.png)

图1 Git log 图

1. 初始化项目，在 `master` 分支中加入了 `master-branch.txt` 文档。
2. 从 `master` 分支中派生 `A` 分支，在 `A` 分支中创建并提交 `a.txt`，接着在 `A` 分支中修改 `master-branch.txt` 并提交。
3. 再从 `A` 分支中派生 `B` 分支，在 `B` 分支中创建并提交 `b.txt`，接着在 `B` 分支中修改  `master-branch.txt` 并提交。
4. 切换到 `master` 分支，执行 `git merge --no-ff A`，把 `A` 分支合并到 `master` 分支中，成功。
5. 切换到 `B` 分支，执行 `git rebase master`，执行结果如图2，按照提示执行 `vim master-branch.txt` 解决冲突如图3。
6. 冲突解决完之后，执行 `git add master-branch.txt`后再执行 `git rebase --continue`, rebase 完成如图4。

![图2](/uploads/tech/git_study_notes/git_study_notes_2.png)

图2

![图3](/uploads/tech/git_study_notes/git_study_notes_3.png)

图3

![图4](/uploads/tech/git_study_notes/git_study_notes_4.png)

图4

---

Git 模拟仓库打包在 [testgit.zip](/uploads/testgit.zip) 中
。


-EOF-