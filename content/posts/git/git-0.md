+++
title = 'Git 中 submodule 的使用'
date = 2023-12-18T17:39:39+08:00
draft = false
categories = ["git"]
tags = ["git", "submodule"]
+++

Git 的 submodule 使一个 git 仓库成为另一个 git 仓库的子库，git submodule 简单来说就是对另一个仓库在特定时间点的引用。git submodule 使得 git 仓库可以整合并追踪外部代码库的版本历史。

## What is a Git submodule?

代码库经常会依赖外部代码。外部代码可以通过不同的方式整合到代码库中。

+ 外部代码可以直接拷贝和粘贴至主仓库中
  + 缺点：失去对外部代码上游变更的追踪
+ 使用包管理系统，例如 Ruby Gems 或者 NPM。
  + 缺点：需要在部署源代码的所有地方进行安装和版本管理

以上两种整合方式都不支持跟踪外部代码的修改和变更。

git submodule 是一个主机 git 仓库的记录，该记录指向外部代码库的特定 commit 。git submodule 是静态的并且只追踪特定 commit ，它不追踪 Git 引用或者分支，也不会在主仓库更新时自动更新。在给一个仓库添加 submodule 时，会创建一个 .gitmodules 文件，记录外部代码库的元数据信息（本地路径以及外部代码URL），如果主仓库有多个 submodule ，.gitmodules 文件 将为每一个 submodule    创建一个条目。

## When should you use a Git submodule?

如果你需要对外部依赖项进行严格的版本管理，使用Git子模块是有意义的。以下是一些适合使用Git子模块的最佳情况：

+ 当外部组件或者子模块版本更新的很快并且未来的改变可能会破坏当前的 api ，你可以为了当前代码库的安全把子模块锁在特定的版本。
+ 当你有一个组件不经常更新，而你希望将其视为供应商依赖项时，可以使用Git子模块。
+ 当你将项目的一部分委托给第三方，并希望在特定时间或发布时集成他们的工作时，可以使用Git子模块。同样，这在更新不太频繁的情况下有效。

## Common commands for Git submodules

### Add Git submodule

`git submodule add` 给当前的 git 仓库添加一个新的子模块。

```
git submodule add https://github.com/adityatelange/hugo-PaperMod.git
```

执行上述命令后会克隆对应的项目并且创建 `.gitmodules` 文件，内容如下。

```
[submodule "themes/PaperMod"]
	path = themes/PaperMod
	url = https://github.com/LucasVaa/hugo-PaperMod
```

### Cloning git submodules

```、
git clone /url/to/repo/with/submodules
git submodule init
git submodule update
```

`git submodule init` 的默认行为是将 `.gitmodules` 文件中的映射复制到本地的 `./.git/config` 文件中。这可能看起来有些冗余，使人质疑 `git submodule init` 的实用性。`git submodule init` 还有扩展行为，它接受一个显式模块名称的列表。这使得可以选择性地激活仅对仓库进行工作所需的特定子模块。如果仓库中有许多子模块但并非所有子模块都需要在进行工作时获取，这将会很有帮助。

### Submodule workflows

一旦子模块在父仓库内得到正确初始化并更新，它们可以像独立的仓库一样被使用。这意味着子模块有它们自己的分支和历史。当对子模块进行更改时，重要的是要发布子模块的更改，然后更新父仓库对子模块的引用。



