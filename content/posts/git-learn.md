---
title: "Git常用命令的学习与理解"
date: 2023-12-13T17:53:55+08:00
type: "posts"
---

## 初始化

```shell
git clone <repository name> #repository name为远端仓库，该操作会把远端仓库克隆到本地，并把执行该克隆命令的文件夹做为git 本地仓库。
```

```shell
git init #指定某个仓库做为git本地仓库，为初始化操作
```

```shell
 git pull <repository name> #repository name为远端仓库 
```

```shell
git pull <repository name> <branch name>#指定拉取该仓库某个分支的代码到本地
```

> `git init` + `git pull -b <branch name><repository name>` 在效果上等同于 `git clone <repository name>`。 但是在初始化的时候更推荐后者。因为后者可以默认与该仓库下所有分支都建立联系。前者只与`git pull -b <branch name> < repository name>`时 的branch name 建立联系。



## 分支 🔀

```shell
git branch uat #新建一个uat分支，但是还在当前分支上，不会切换到uat分支
```

```shell
git branch -d <branch name>#删除本地分支
```

```shell
git push <remote_name> --delete <branch_name> #删除远端分支<remote_name>为远端仓库一般是origin <remote_name>为分支名字
```

```shell
git checkout uat #从当前分支将分支切换到uat分支
```

```shell
git checkout -b uat#新建一个uat分支，并切换到uat分支
```

```shell
git checkout <commit-id>#命令切换到某个特定的提交，这相当于创建一个临时分支。
```

```shell
git branch -u <remote-name>/<remote-branch> #sa本地分支与远程分支进行关联
```

> 如果在dev分支进行操作时，uat有bug需要checkout到uat，但是不想对dev的修改进行commit，可以用`git stash` 将修改放到git的隐藏栈中，Git 会创建一个临时的提交对象来保存这些更改，同时清空工作目录和暂存区，以便你可以在干净的状态下进行其他操作。
>
> `git stash save`会将未提交的更改保存为一个临时的提交对象，并将 HEAD 指针移动到最新的提交。这个临时提交对象包含了你的工作目录和暂存区的更改。**这个临时提交不属于任何分支，而是作为一个独立的提交存在**。



```shell
git stash save <message> #message为对此次暂存进行描述。过期
git stash push -m "message"
```

```shell
git stash list #查看所有暂存
```

```shell
git stash apply stash@{n}  与⬆️搭配使用，将该分支代码恢复到某个暂存索引。
```

```shell
git stash apply #进行将刚刚的暂存应用到当前分支。
```

```shell
git stash pop #应用后将此次暂存删除
```

> 该应用为应用到本地的分支上



## 好用的标签 🏷️

```shell
git tag <tag name> #给当前的版本改动打一个标签
```

> 该标签对所有分支有效，是对该分支**已经commit**的代码打tag

```shell
git tag #查看本地的标签
```

> 标签需要被打标签者push后，协作者👨🏻‍💻拉取该标签后才可以用此命令看到

```shell
git tag -d <tag name># 删除本地标签，<tag name>为标签名
```

```shell
git push <repository name> --tags # 将本地所有标签推送到远端
```

```shell
git push <repository name> <tag name>#将本地指定标签上传到远端
```

```shell
git push origin --delete <tag_name> #删除远端某一标签
```

```shell
git fetch -- tags#拉取所有远端tag
```

<span style="color:lightgray">*我上传了一个tag A，同事也将此tag pull到本地，然后我将该tag进行本地与远端均删除。 重新上传一个新的同样名字的tag A，这时，同事没有pull代码，就用tag 新建了一个分支，这时他用的是之前的tag。*</span>

```shell
git branch <branch name> <tag nmae> #将tag内容新建一个分支
```

## 合并🈴️

```shell
git merge <branch name> #将引用里的分支名合并到当前分支
```

> 相对本地操作

> 1. 普通合并（recursive）： 这是 Git **默认**的合并策略，也是最常用的策略。它会尝试进行三路合并，自动合并非冲突的修改，并在有冲突的地方产生冲突标记，需要手动解决。例如：
>
>    ```shell
>    git merge --strategy=recursive feature-branch
>    ```
>
> 2. 快进合并（fast-forward）： 当合并的两个分支形成线性关系时，且目标分支没有新的提交，可以使用快进合并。快进合并会将目标分支指针直接移动到要合并的分支的最新提交，相当于没有产生新的合并提交。例如：
>
>   ```shell
>  git merge --strategy=fast-forward feature-branch
>   ```
>
> 3. 递归合并（octopus）： 递归合并策略用于同时合并多个分支。它可以一次性将多个分支的修改合并到当前分支，并生成一个新的合并提交。递归合并常用于同时合并多个特性分支或多个远程分支。例如：
>
>    ```shell
>     git merge --strategy=octopus feature-branch1 feature-branch2
>    ```
>
> 4. 简单合并（resolve）： 简单合并策略是一种比较保守的策略，它会直接将两个分支的修改进行比较，并尝试自动合并。如果有冲突，它会停止合并并提示冲突，需要手动解决。简单合并适用于简单的合并场景。例如：
>
>   ```shell
>  git merge --strategy=resolve feature-branch
>   ```

## 撤回⏪

### add后

```shell
git reset HEAD <file> #撤销对某一文件的add
```

### commit后

```shell
git log #查看记录，会显示唯一确定一次commit的标识码
```

```shell
git revert <commit num> #创建一个新的提交，用于撤销指定的commit; 还可以用于push后的撤回
```

#### git reset 的三种模式与区别

```shell
git reset —soft <commit num> 
#会将 HEAD 指针和分支引用移动到目标提交，但不会更改工作目录和暂存区的内容。这意味着之后可以重新提交或修改之前的提交。理解为温柔的回退,执行该操作后，commit前修改的记录还在暂存区，不需要重新add。
```

```shell
git reset —mixed commitnum
#默认方式，这种模式下，会将 HEAD 指针和分支引用移动到目标提交，并重置暂存区的内容，但不会更改工作目录的内容。这意味着之后需要重新添加和提交更改。执行该操作后，重置暂存区内容，也就是此次变更需要重新add到暂存区。
```

```shell
git reset —hard commitnum
#这种模式下，会将 HEAD 指针和分支引用移动到目标提交，并且完全重置工作目录、暂存区和之后的更改。这意味着之后的更改将被丢弃，请确保在执行此操作之前保存好重要的更改。
```

> Ex:
>
> 新建A.txt，add后commit -m "generate A.txt"（commit号为 aa1）
>
> 此时，A.txt 文件中有两条记录 ：Test1 Test2
>
> 变更A.txt ，删除字段Test2，add后commit -m "delete Test2" (commit号为bb2)
>
> 执行git reset 到 aa1 
>
> `git reset --<model> aa1`
>
> * soft模式下，add暂存区有Test2的删除记录，工作区A.txt 为Test1
>
> * mixed模式下，add暂存区无Test2的删除记录，工作区A.txt为Test1，需要重新将删除 Test2的操作add到暂存区。
>
> * hard模式下，add暂存区无Test2的删除记录，工作区为Test1 Test2，相当于上一次commit "generate A.txt"后，什么也没有发生。

### push后

```shell
git revert <commit num> #创建一个新的提交，用于撤销指定的commit; 还可以用于push后的撤回
```

> 执行此指令后，Git 会自动打开一个编辑器，需要编写撤销提交的相关信息。可以修改信息后保存并关闭编辑器。

## 远程仓库操作相关

### 查看与删除

```shell
git remote # 查看与本地仓库关联的远程仓库信息，如远程仓库的名称;
git remote -v #查看远端仓库的详细URL等
```

```shell
git remote add <repository name> <url>#将一个新的远程仓库添加到本地仓库的远程仓库列表中
```

```shell
git remote remove <repository name>#从本地仓库的远程仓库列表中移除指定的远程仓库。
```

### 拉取

```shell
git pull #从远程仓库获取最新的代码更新，并将其合并到当前分支
```

```shell
git fetch #从远程仓库获取最新的分支信息，但不会自动合并到本地分支。需要通过切换到远程分支或进行合并操作来更新本地分支。
```

```shell
git push --force #即使它会覆盖远程仓库中的提交。请谨慎使用此选项，因为它可能会导致数据丢失或不一致
```

### 推送

```shell
git push <repository name> <branch> #将指定的本地分支推送到远程仓库。<remote> 是远程仓库的名称，<branch> 是本地分支的名称。如果已经与远端仓库关联，可直接使用 `git push`
```

```shell
git push -u <repository name> <branch> #将本地分支推送到远程仓库，并将远程分支设置为该本地分支的上游分支。这样可以建立本地分支与远程分支之间的跟踪关系，以后可以使用 git pull 或 git push 简化命令。
```

## 高级重写操作

```shell
git commit --amend #修改提交消息
```

```shell
git rebase -i #修改提交消息、合并提交、删除提交等
```

```shell
git rebase -i HEAD~3 #修改最近的三次commit 会打开一个编辑器，可更改文件内容，将文件前的操作命令进行修改，以达到修改commit的目的

#drop：删除提交，类似于 d。
#edit：编辑提交，允许你在重新应用提交之前对提交进行修改。
#reword：修改提交信息，类似于 r。
#fixup：合并提交，但不保留提交信息，类似于 f。
#squash：合并提交，保留提交信息，类似于 s。
#exec：执行指定的 shell 命令。
```

> 删除某个提交后，工作区和暂存区完全回退
>
> 与`git reset --hard`不同的是，前者重写后的提交历史将被修改，原始提交将被移除。后者只能回退到某一特定提交，这一特定提交之前的提交也会被删除。
>
> `git rebase -i` 更适合对提交历史进行高级编辑，而 `git reset --hard` 更适合重置当前分支的状态到某个特定的提交。



## 一些缩写以及含义

- `-u` 或 `--set-upstream`: 将本地分支与远程分支建立关联关系。
- `-v` 或 `--verbose`: 显示详细的输出信息，更多地展示命令执行过程。
- `-a` 或 `--all`: 显示所有分支（包括远程分支和本地分支）。
- `-f` 或 `--force`: 强制执行操作，覆盖现有的对象或分支。
- `-m` 或 `--merge`: 执行合并操作。
- `-r` 或 `--remote`: 显示远程仓库的相关信息。
- `-p` 或 `--patch`: 以交互式方式显示并选择要提交的文件的补丁。
- `-s` 或 `--short`: 显示简短的输出信息，只展示关键信息。



## 理解Git的原理

Git 使用一种称为对象存储（Object Store）的机制来管理文件的内容和历史记录。对象存储是 Git 的核心组成部分，它负责存储所有文件的内容和元数据，并以唯一的哈希值来标识每个对象

1. Blob（文件对象）：Blob 对象代表着 Git 仓库中的文件内容。它保存着文件的数据，并且通过 SHA-1 哈希值来标识。每次你对文件进行修改并提交时，Git 会为该文件创建一个新的 Blob 对象来保存修改后的内容。
2. Tree（目录对象）：Tree 对象代表着 Git 仓库中的目录结构。它保存着目录的信息，包括文件名、文件类型和对应的 Blob 或 Tree 对象的哈希值。通过递归地组合 Tree 对象，Git 可以构建出完整的目录结构。
3. Commit（提交对象）：Commit 对象代表着 Git 仓库中的一个代码提交。它包含了作者、提交时间、提交信息以及指向当前提交所对应的树对象的指针。每次你执行 `git commit` 命令时，Git 会创建一个新的 Commit 对象，并将当前的工作目录状态转化为一个树对象。

*举个例子，假设你的 Git 仓库中有一个名为 `src` 的目录，其中包含了 `main.c` 和 `util.h` 两个文件。在提交代码时，Git 会将 `src` 目录转化为一个 Tree 对象，其中包含了对应的 Blob 对象（文件对象）。当你修改了 `main.c` 文件后，Git 会创建一个新的 Blob 对象来保存修改后的内容，并更新 Tree 对象中对应的文件哈希值。然后，Git 会创建一个新的 Commit 对象，其中包含了指向最新的树对象的指针。这样，你就可以通过 Commit 对象来访问到该次提交所对应的文件对象。*

### 引用

分支引用、标签引用、远程引用 三种不说明

#### 一种特殊引用HEAD

当 `HEAD` 指向分支时，它指向该分支的最新提交；当 `HEAD` 指向具体的提交时，它直接指向该提交。

如当前在main分支，head指向为main分支的最新的一次提交，checkout branch dev时，head指向dev分支的最新的一次提交。

在reset 到某个commit num时，head指向该提交。



