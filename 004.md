# 如何保持 GitHub Fork 项目的同步

原文：[Keeping GitHub Forks in Sync](https://github.com/LambdaSchool/CS-Wiki/wiki/Keeping-GitHub-Forks-in-Sync)

作者：[beejjorgensen](https://github.com/beejjorgensen)      发表时间：2018/1/13

译者：[陈 昌茂](https://juejin.im/user/59aabc9af265da249517aa6d)                   发表时间：2018/8/29

(转载请注明出处)



很多时候，你在 GitHub 上 `Fork` 了某个上游项目，还希望同步原项目作者在此后所做的变更。

虽然用户很早就提交了 [功能提议](https://github.com/isaacs/github/issues/121)，但GitHub 到目前为止还不支持在 Web 界面上的同步操作。遗憾的是，可能以后也不会。

这很正常。本文通过命令界面操作，几条命令可以就解决项目同步的问题。

## 前言

为方便行文，我们将项目的三个别名介绍如下：

* `origin`：代表远程项目，即 GitHub `Fork` 项目（克隆自上游项目，即 `upstream`）
* `local` 代表本地项目，即电脑上的本地项目（克隆自远程项目）
* `upstream`：代表上游项目，即被 `Fork` 的 GitHub 项目。

有两种方法使 `origin` 与 `upstream` 保持同步：

1. 提交合并建议（PR）给 `upstream` ，上游项目的作者能在 GitHub 的 Web 界面上操作是否接受你的 PR。**当然这通常很少发生**，因为通常有成百上千的人克隆了他的项目，他不得不这么做。（译者认为「有用」的PR一般是会被接受的）
2. 把 `upstream` 的更新获取并合并到 `local` ，然后推送到你的 `origin` 。**这是最常用的方法。**

![GitHub Fork 项目](https://user-gold-cdn.xitu.io/2018/8/29/1658638f745dbc8a?w=605&h=385&f=png&s=25550)

## 什么是 `upstream`

**`upstream` 是很重要的设置，只需进行一次。**

`upstream` 是远程项目地址的别名，代表一个远程项目的  URL  地址（类似 DNS 之于 IP 地址，可用 `git remote` 查看，译者注）。虽然可以每次在引用远程项目的时候都使用完整的  URL  地址，但这是一个痛苦的体验，所以我们用「别名」。

如你所见，`origin` 也是一个远程项目的别名，在克隆项目的时候 `git` 命令会自动设置为被克隆项目 URL 地址。

如果你已经克隆了一个项目，可以使用 `git remote -v` 命令来查看。

```bash
$ git clone git@github.com:MyName/My-Forked-Repo.git
[...cloning output...] 

$ git remote -v
origin	git@github.com:MyName/My-Forked-Repo.git (fetch)
origin	git@github.com:MyName/My-Forked-Repo.git (push)
```

可以设置多少个远程项目别名，因为它们仅仅代表一些远程项目的 URL 地址而已。让我们来加一个「引用 」到上游项目，别称为 `upstream`。

```bash
$ git remote add upstream https://github.com/LambdaSchool/Original-Repo.git

$ git remote -v
origin	git@github.com:MyName/My-Forked-Repo.git (fetch)
origin	git@github.com:MyName/My-Forked-Repo.git (push)
upstream     https://github.com/LambdaSchool/Original-Repo.git (fetch)
upstream     https://github.com/LambdaSchool/Original-Repo.git (push)
```

现在可以看到有2个远程项目别名。

你可能注意到，远程项目 URL 地址是 HTTPS 的。这是因为我们（大概）没有上游项目的 SSH 访问权限，但这没关系，因为我们只需要读取权限即可，并不需要写入权限。

## 同步你的 `Fork`

如上图所示，我们把 `upstream` 的更新获取并合并到 `local` ，然后推送到  GitHub 上的 `origin` 。操作完成后，`local` 和 `origin` 就与`upstream` 保持同步了。

**下述操作，是假设将 `local` 的 `master` 分支与 `origin` 的 `master` 分支合并。如果合并其他分支，调整成对应的分支即可。**

### 从 `upstream` 获取更新

命令 `git fetch` 与 `git pull` 功能一致，区别是前者只获取不合并。

> 有趣的是： `git pull` 即是 `git fetch` 命令和 `git merge` 命令的简写。


执行如下命令，从 `upstream` 获取更新：

```bash
$ git fetch upstream
[...fetch output...]
```

### 合并 `master` 分支与 `upstream/master` 分支

上一步获取了 `upstream` 的所有更新。这一步是如何合并这些更新。

1，执行如下命令，查看本地是否已经提交了全部变更。

```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```

如果提示有暂存的变更，请参考[这](https://git-scm.com/book/en/v1/Git-Tools-Stashing)提交之后再继续。

2，确保切换到本地的 `master` 分支，因为 `master` 分支是准备要合并的分支。（*号表明是当前的分支）

```bash
$ git checkout master

$ git branch
* master
```

3，执行合并操作。下述命令中的 `upstream/master` 指 `upstream` 的 `master` 分支。（区别本地的 `master` 分支）

```bash
$ git merge upstream/master
[...merge output...]
```

注意，如果提示输入修订日志，请输入「合并分支」或任何其他内容。（如果是 ` fast-forward` 方式合并，无需输入修订日志）

如果在合并过程中没有看到任何冲突提示信息，合并成功，那这会是合并过程中最开心的时刻。

否则在进行下一步前，先解决合并冲突。限于篇幅本文不涉及 [如何解决合并冲突](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)，但那会也不会是世界末日。

一个有用的快捷命令 `git up`，如何设置请查阅 [git-config](https://git-scm.com/docs/git-config) 文档。

>   `up = "!git remote update -p; git merge --ff-only `[`@{u}`](https://git-scm.com/docs/gitrevisions)`"`

使用快捷命令或多或少会使人忘记如何正确的使用 `git pull` 命令──不像 `git pull`，`git up` 永远不会让你到一个提示，希望你解决一个合并冲突。

> `git up` 从所有上游分支下载所有最新提交（修剪死分支），并尝试将本地分支快速转发到上游分支上的最新提交。如果成功，则没有本地提交，因此没有合并冲突的风险。如果存在本地（未刷新）提交，则快进将失败，从而可以在执行操作之前查看上游提交。

### 上传到 `origin`

在获取和合并操作后，本地项目已经和上游项目保持同步。要使远程项目也保持同步，还得执行如下命令：

```bash
$ git push
[...push output...]
```

至此，此次同步工作完成！