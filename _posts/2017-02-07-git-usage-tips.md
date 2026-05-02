---
layout: post
title: Git 使用注意点
date: 2017-02-07 20:30:00 +0800
categories:
- Tech
tags:
- Productively
- Git

---

## Git 四个重要的名词

- 工作副本
- 暂存区
- 本地仓库
- 远程仓库


## 常用命令

- 拉取分支。`git pull` 这个命令将会从远程分支下载所有的新的提交到你的本地副本中来。它实际上就是一个 “抓取（fetch）” 命令（下载数据） 和 一个 “ 合并（merge）” 命令（整合那些下载的数据到你的本地副本）的组合。和 “git push” 命令一样，如果你本地的 HEAD 分支还没有创建任何一个 “跟踪” 链接，你就必须告诉 Git，你要从哪一个远程仓库上的哪一分支中抓取数据（例如 “git pull origin master”）。如果已经存在了一个链接，只需要简单键入 “git pull” 就足够了.
- 发布一个本地分支。`git push -u origin contact-form` 中 `-u` 会创建本地分支与远程分支的“跟踪”关系。
- 删除分支。删除本地 `git branch -d contact-form`，删除远程仓库中的分支 `git branch -dr origin/contact-form`。
- 修改最后一次提交。`git commit --amend -m "This is the correct message"`。然而要记住一点：`不要修改已经被发布的提交`。
- 撤销本地改动。恢复一个文件到上次提交之后的状态，执行 `git checkout -- file/to/restore.ext`；放弃本地工作副本(working copy)，执行 `git reset --hard HEAD`。
- 撤销已提交的改动。`git revert 2b504be`，这个命令会产生一次提交操作；使用 `git reset --hard 2be18d9` 这个命令上使用 “--hard” 参数则一定要小心，Git 将会丢弃所有你当前可能拥有的本地改动。整个项目将会被恢复成一个之前的旧版本。如果你使用 “--keep” 参数来替代 “--hard” 参数，那么在 “回滚” 到的版本之后的所有改动将会转换成本地改动，并保留在你的工作目录中。和 “revert” 命令一样， “reset” 命令也不会删除任何已存在的提交。而无论如何，提交会被保存在 Git 的数据库中至少30天。
- [读懂 Diffs](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/diffs#start)
- 检查本地改动。`git diff`
- 比较分支和版本。`git diff master..contact-form` 和 `git diff 0023cdd..fcd6199`。
- [处理合并冲突](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/merge-conflicts#start)。撤销一个合并，输入 `git merge --abort` 命令，你的合并操作就会被安全的撤销。`git reset --hard` 也能完成相似的功能。
- Rebase 代替合并。虽然合并（merge）操作可以用来简单和方便地整合改动，但是它却不是唯一的方法。“Rebase” 就是另一种替代手段。`MERGE` 会查找三个提交做为整合的目标，**共同的原始提交**以及**两个分支的最终点**。合并过程[请看这里](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/rebase#start)。Rebase 整合会**rebase 会改写历史记录**。通过 `rebase` 之后的最新的提交实际上已经不再是 `rebase` 之前的提交，因为**历史记录被改写了，提交的顺序变了**。要注意的是，如果你重写了已经发布到公共服务器上的提交历史，这样做就非常危险了。其他的开发人员可能这时已经在最原始的提交 C3 上开始工作，并使它成为了一些新提交中不可或缺的部分，而现在你却把 C3 的改动设置到了另一个时间点（就是那个新的 C3*）。除此之外，通过rebase 操作，这个原始的 C3 还被删除掉了，这将是非常可怕的……因此你应该只使用 rebase 来清理你的本地工作。
- 子模块(Submodule)。一个 “子模块” 其实就是一个标准的 Git 仓库。一个子模块也是一个功能齐全的 Git 仓库，就内部而言它和别的仓库没有什么区别，你可以对它进行修改、提交、抓取、推送等等操作。执行 `git submodule add https://github.com/djyde/ToProgress` 命令增加一个子模块。一个子模块的内容并不保存在它的父仓库中，只有它的远程 URL 会被记录在父仓库中，以及它在主项目中的本地路径和签出的版本，子模块并不是主项目的版本控制的一部分。子模块的配置会存储在主项目的 `.gitmodules` 文件中(不建议手动修改这些配置文件，采用 Git 命来操作)。需要删除一个子模块时执行 `git submodule deinit lib/ToProgress`、`git rm lib/ToPogress` 即可。
- [git-flow 的工作流程](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow#start)[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
- [使用 SSH 公钥验证](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/ssh-public-keys#start)。访问远程服务器上的 Git 仓库时，进行有效的认证是非常必要地。你可能已经通过你所使用的浏览器了解了 “HTTPS” 协议，尽管它使用起来很简单，但是很多系统管理员还是会出于各种原因去选择使用更为普遍的 “SSH” 协议。在这种协议之下，当涉及到身份验证时，你就很可能需要 “SSH公钥”。对于这种类型的验证需要一对密钥：一个公钥和一个私钥。私钥，顾名思义就是必须在任何情况下都保持绝对私有。它所对应的公钥则相反，应该是被安装到那些你需要登陆的服务器上。当通过 SSH 试图建立连接时，如果客户端提供的私钥能和在服务器上所安装的公钥相匹配，那么这个客户端才会被授予访问权限。


## Git使用注意点

Git 教程：

- [Getting Git Right](https://www.atlassian.com/git) - Atlassian
- [Learn Version Control with Git](https://www.git-tower.com/learn/) - Tower
- [git - the simple guide](http://rogerdudler.github.io/git-guide/)

Git 命令：

- [git-cheat-sheet](https://github.com/jakubpawlowicz/git-cheat-sheet/blob/master/README.md)


[分支的工作流程](https://www.git-tower.com/learn/git/ebook/cn/command-line/branching-merging/branching-workflows#start)

## (A) 短期分支（Short-Lived）/主题分支（Topic Branches）

1. 这些分支的只涉及到一个主题
2. 它们的生命周期都比较短

## 长期分支 Long-Running Branches

1. 不应该直接在这个分支上工作
2. 长期分支分可能存在不同的等级。production、development

### 分支策略

1. 只使用一个长期分支。
2. 主题分支的合并。所有被合并到 “master” 分支的代码都必须保证正确。使用单元测试（unit tests），代码审查（code reviews）等等来确保分支的准确性。
3. 保持远程同步。
4. 频繁推送。保持与远程分支的同步并不是只停留在结构层面上，经常性地通过 “git push” 命令发布你的改动可以有助于团队里的其他的开发人员看到和使用你的最新开发成果。附带的还有一个最大的好处是，它可以作为你的远程备份。


-----

## 版本控制的最佳实践

1. 提交对映改动.一次提交要包括一个相关改动，对于两个错误的修复应该进行两次不同的提交。
2. 频繁地提交改动。经常性地提交改动可以确保不会出现特别庞大的提交，同时也可以比较精准地对应到所需要的改动上。
3. 不要提交不完整的改动。虽然原则上来说不要提交一些还没有完成的改动，但是对于一个非常庞大的新功能来说，也并不意味着你必须整体完成这个功能后才可以提交。你必须把那些改动正确地分割成一些有意义的逻辑模块来进行频繁地提交。
4. 提交前测试那些改动。不要理所当然地认为自己完成的改动都是正确的。所有的改动一定要通过彻底地测试才表示它真正地被完成了。
5. 高质量的提交注释。提交注释的标题需要一个少于50个字符的简短说明。在一个空白的分割行之后要对改动的细节进行一个详细地描述。
6. 版本控制不是备份系统。版本控制系统具有一个很强大的附带功能，那就是服务器端的备份功能。但是千万不要把 VCS 仅仅当成一个备份系统。
7. 使用分支功能。分支是 Git 一个非常强大的功能，当然这不是偶然的。自始至终，Git 的宗旨就是提供一个即快速又简单的分支功能。比如添加新功能，修复错误，尝试新的想法等等。
8. 遵循一个工作流程。Git 可以支持很多不同的工作流程：长期分支、功能分支、合并以及 rebase、git-flow 等等。选择什么样的开发流程要取决如下一些因素：项目开发的类型，部署模式和（可能是最重要的）开发团队成员的个人习惯。不管怎样，选择什么样的流程都需要得到所有开发成员的一致认可，并且一直遵循它。


----

相关资料：

[从 Subversion 过渡到 Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/from-subversion-to-git#start)

[为什么选择 Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/why-git#start)
