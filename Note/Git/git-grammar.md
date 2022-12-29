# 版本控制工具

## 集中式

> CVS、SVN（Subversion）、VSS

有一个单一的集中管理的服务器，保存所有文件的修改版本。所有协同工作的人都连接到这台服务器操作文件。

### 好处

- 每个人都能看到其他人在做什么
- 管理员可以很方便的管理每个人的权限以及管理这台服务器

### 缺点

- 如果这台服务器故障，团队基本就瘫痪了
  - 所有人都不能对代码进行版本控制了

## 分布式

> Git、Mercurial、Bazaar、Darcs

每个协同者从仓库拉取时，是将完整的、每个版本都镜像到本地仓库中。

### 好处

- 版本控制是在本地完成的
  - 即便远程仓库出问题，也可提交到本地，待远程仓库正常运行时再提交到远程仓库
- 即使远程仓库坏掉了，也可从本地仓库进行恢复
  - 每个拉取了远程仓库的主机上都有完整的历史版本和最新版本



# Git

## 命令
