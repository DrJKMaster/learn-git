# git

## 工作机制

### 基本

|    远程库    |    本地库    |  暂存区  | 工作区 |
| :----------: | :----------: | :------: | :----: |
| 远程代码仓库 | 本地代码仓库 | 临时存储 | 代码区 |

### 原理

#### index

- 存储文件名信息

#### objects

- blob
  * 每个文件的不同版本在 git 中存储为一个二进制对象，不存储文件名，相同内容的文件会关联到同一个二进制对象
  * blob 文件是经过压缩的
- commit
  - 提交时产生的文件
  - 包含：文件树，作者信息，时间等
- tree
  - 文件树
  - 包含：文件名及 blob 对象，下一级文件夹及 tree 对象
- tag
  - 标签


### HEAD

- HEAD
  - 指向当前分支（一个文件，文件名与分支相同，存储了当前分支所使用的版本的指针）
- FETCH_HEAD
  - 当使用 pull 或 fetch 命令时更新
- ORIG_HEAD
  - merge 后生成，搭配 reset 方便回到 merge 前状态


### 标签

- 显示所有标签

  ```bash
  git tag
  git tag -n （同时显示注解）
  ```

- 轻标签

  ```bash
  git tag 标签名          （给当前 commit 打标签）
  git tag 标签名 提交编号   （给目标 commit 打标签）
  ```

  - 在本地暂时使用或一次性使用

- 注解标签

  ```bash
  git tag -a 标签名 -m "注解"
  ```

  - master 分支发布时使用

- 删除标签

  ```bash
  git tag -d 标签名
  ```

  

## bash

### 初始化

```bash
git config --global user.name 用户名
```

- 设置用户签名

```bash
git config --global user.email 邮箱名
```

- 设置用户签名

```bash
git init
```

- 初始化本地库（在想要创建本地库的目录下执行）

### 分支内操作

```bash
git status
```

- 查看本地库状态

```bash
git add 文件名
```

- 将文件添加到暂存区（有文件被修改则需要执行，否则无法切换分支）

```bash
git commit -m "日志消息"
git commit --amend -m "日志消息" （修改上一次提交）
```

- 将文件提交到本地库

### 分支间操作

```bash
git branch 分支名
```

- 创建分支

```bash
git branch -v
git branch -r （查看远程仓库）
```

- 查看分支

```bash
git checkout 分支名
git checkout -b 分支名 （创建并切换分支）
```

- 切换分支

```bash
git merge 分支名
--squash （将被合并的分支上的多个 commit 变为一个 commit 后再合并）
--no-ff （不要 fast-forward）
```

- 将指定的分支合并到该分支上

  - fast-forward

  - 3-way-merge

    - 3-way-merge 产生的线路图会更加清晰，但有时我们想要一条线的提交记录，则可在 merge 前使用 git rebase

      ```bash
      git rebase 要被 merge 到的分支
      解决冲突
      git rebase --continue 解决下一个冲突，或者 rebase 结束
      git rebase --abort 取消 rebase，复原
      ```

      - 不要使用该命令对已提交到远程仓库的 commit 进行修改
      - 将该分支的起点移动到主分支的最新 commit 节点后


```bash
git diff 分支1 分支2 --stat  //显示出所有有差异的文件列表
git diff 分支1 分支2 文件路径  //显示指定文件的详细差异
git diff 分支1 分支2          //显示出所有有差异的文件的详细差异
```

- 显示分支间的差异
  - 只写一个分支：查看当前工作区和分支的差异
  - 不写分支：查看当前工作区和当前分支的差异


```bash
git branch -D 分支名 （强制删除）
git branch -d 分支名 （删除前会检查是否被 merge）
```

- 删除分支

```bash
git log（查看日志）
--oneline（简洁版）
--graph（显示拓补图）
--reverse（逆向显示）
--author=作者名（查看某作者的提交记录）
--decorate（查看带标签信息的日志）

git reflog（查看简化日志）
```

- 查看日志

```bash
git reset --hard 版本号（在 reflog 中查看）
```

- 版本穿梭

### 团队协作

- 通过远程仓库 Github Gitee Gitlab 等进行团队协作

```bash
git remote add 远程库别名（推荐为 origin） 远程库地址（可以为 http 或 ssh）
```

- 添加远程仓库

```bash
git remote -v
```

- 查看远程仓库

```bash
git remote show 远程仓库名
```

- 查看远程仓库信息

```bash
git push 远程库别名|远程库地址 本地分支名:远程分支名
-u 将该分支推送到远程仓库，如果远程仓库没有该分支则自动创建，最后将此分支关联到远程仓库分支
-d 删除远程分支
```

- 推送分支到远程仓库
- ":远程分支名"可省略，表示推送到远程仓库的同名分支
- push 之前如果发现有版本问题，则应该先 pull 不然可能会因为版本问题无法 push
- 特殊用法：如果省略"本地分支名"，则表示将空分支推送到远程仓库，即删除远程仓库的分支

```bash
git pull 远程库别名|远程库地址 远程分支:本地分支
```

- 拉取分支到本地仓库并 merge（推荐使用 fetch + merge，更安全）

```bash
git fetch 远程库别名|远程库地址 远程分支:本地分支
```

- 拉取分支到本地仓库

- ":本地分支名"可省略，表示拉取到本地分支"远程仓库名/远程分支名"

- 远程仓库删除了某分支，本地执行 fetch 也不会删除

  ```bash
  git remote prune
  或
  git fetch --prune
  ```

  - 删除多余的远程分支

```bash
git clone 远程库地址
```

- 克隆到本地仓库，不需要 git init 操作，同时为远程库地址创建别名 origin

### 其他

```bash
git rebase -i 提交编号1 提交编号2
```

- 将多个 commit 合并为一个 commit
  - 提交编号1：想要合并的 commit 中第一个 commit 的前一个 commit 编号
  - 提交编号2：想要合并的 commit 中最后一个 commit 编号
    - 提交编号2可省略，表示合并到当前分支的最后一个 commit

```bash
git cherry-pick 提交编号                 将某个提交应用到该分支
git cherry-pick 提交编号1 提交编号2        将某个两提交应用到该分支
git cherry-pick 提交编号1..提交编号2       将某段提交应用到该分支（不包含提交编号1）
git cherry-pick 提交编号1^..提交编号2      将某段提交应用到该分支（包含提交编号1）
git cherry-pick --continue              解决冲突后继续
git cherry-pick --abort                 取消 cherry-pick，复原
```

- 将某些提交应用到该分支
  - 不想应用其他分支的全部提交（merge）时使用

```bash
git blame 文件名
```

- 查看指定文件的修改记录

```bash
git --version
```

- 查看 git 版本

```bash
git gc
```

- 压缩 objects 文件（如果有多个 blob 文件内容相似，占用空间很大，则可用此方法大幅压缩）

```bash
git brune
```

- 删除未被引用的垃圾文件

- 能够删除多次 add 生成的垃圾文件

- 无法删除当强制删除一个分支后遗留的 commit 及其附属文件

  ```bash
  git \
  -c gc.reflogExpire=0 \
  -c gc.reflogExpireUnreachable=0 \
  -c gc.rerereresolved=0 \
  -c gc.rerereunresolved=0 \
  -c gc.pruneExpire=now \
  gc "s@"
  ```

  - 强制删除



## 分支冲突

- git merge 分支：发生冲突，需要手动解决冲突

- 修改冲突的文件
- git add 文件名
- git commit -m "日志消息" 这里不需要写文件名

## hooks

- 钩子：在执行某个动作前/后进行的操作
- 创建 git 仓库时会生成一个 hooks 文件夹，将 .sample 删去就可以使用，本质是 shell 脚本（也可以是 Python）

- 生成的 hook 只对自己有用，如果想要团队中的其他人也能使用 hook，则要用不同的编程语言来实现
  - 不作拓展

## 需要临时切换分支

```bash
git add        （添加到暂存区）
git stash      （保存）
git stash pop  （继续工作）
```



# git flow

## 分支

### master

- 主分支【只读唯一分支，只能从其他分支合并】
- 目的：稳定的发布分支
- 来源：从 release 分支合并
- 去向：无
- 其它：每个版本都需要标签

### develop

- 主开发分支【只读唯一分支，只能从其他分支合并】
- 目的：存放最新的通过 code review 并通过自动化测试的代码
- 来源：从 feature 分支合并
- 去向：release 分支

### feature

- 功能开发分支【可同时存在多个；临时分支，使用完毕可删除】
- 目的：用于新需求新功能的开发
- 来源：从 develop 分支创建
- 去向：develop 分支（未正式上线之前不推送到远程**中央**仓库，防止有 bug 影响他人开发）

### release

- 测试分支【临时分支，使用完毕可删除】
- 目的：进行功能测试，测试过程中发现的 bug 直接在该分支修复
- 来源：从 develop 分支创建
- 去向：develop/master 分支并推送

### hotfix

- 补丁分支【临时分支，使用完毕可删除】
- 目的：对线上版本进行 bug 修复
- 来源：从 master 分支创建
- 去向：develop/master 分支并推送

## 其他工作流

### github flow

- 更加敏捷的工作流
- 支持 CI/CD



# github

## 远程仓库

推荐远程仓库名与本地仓库名保持一致



## 团队间协作

- B 找到需要协作的项目，点击右上角的 fork，项目就会被加载到 B 的远程仓库
- B 开发完成后，发起一个 pull request 请求
- A 收到请求后，可以进行 merge request 合并代码



- 持续的团队间协作，可以配备两个别名
- origin 指向 fork 的仓库
- upstream 指向主仓库



# 公钥

```bash
ssh-keygen -t rsa -C 邮箱 默认存储在C:\Users\用户名\.ssh
ssh-keygen -t rsa -C 邮箱 -f 存储位置（需要先创建需要的文件夹）
```

通过 rsa 算法生成公秘钥对
