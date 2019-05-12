# 修补

## 变基 git rebase

> Reapply commits on top of another base tip
>
> 另一个基本提示上重新应用提交

如果需要获得更清晰的修订历史记录，可以使用 `git rebase` 命令集成分支。

假设我们有两个分支，其中包含 non-fast-forward 方案。

![Rebase01](./snapshots/rebase_branch_001.png)

执行 rebase 命令将导致分支历史记录看起来类似于下面的图示。

![Rebase01](./snapshots/rebase_branch_002.png)

当您将错误修复分支重新绑定到主分支时，将修复来自错误修复分支的提交并将其附加到主分支的末尾。最终结果是 bugfix 分支历史记录一个简单提交流。

如果在附加提交时发生代码冲突，Git 会要求您在继续重新设置其他提交之前修复冲突。

![Rebase01](./snapshots/rebase_branch_003.png)

![Rebase01](./snapshots/rebase_branch_004.png)

Rebase 不会移动 master 分支的位置。在任何情况下，您都可以在 rebase 后执行从 bugfix 到 master 的 fast-forward 或清除合并。

### 变基至主分支

📍**示例：**

```bash
# 切换至次分支
$ git checkout feature

# 变基至主分支
$ git rebase master
```

整个 feature 分支上的提交会移动到 master 分支的后面，有效地把所有 master 分支上新的提交并入过来。

但是，rebase 为原分支上每一个提交创建一个新的提交，重写了项目历史，并且不会带来合并提交。

### 交互式变基

交互式变基允许你更改并入新分支的提交。这比自动的变基更加强大，因为它提供了对分支上提交历史完整的控制。一般来说，这被用于将 feature 分支并入 master 分支之前，清理混乱的历史。

📖 **语法**

```bash
$ git rebase -i [startpoint] [endpoint]
```

 📍 **示例：**

```bash
$ git checkout feature
$ git rebase -i master
```

它会打开一个文本编辑器，显示所有将被移动的提交：

```bash
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

* pick：正常选择
* reword：选中，并且修改提交信息
* edit：选中，rebase 时会暂停，允许你修改这个 commit
* squash：选中，会将当前 commit 与上一个 commit 合并
* fixup：与 squash 相同，但不会保存当前 commit 的提交信息
* exec：执行其他 shell 命令

忽略不重要的提交会让你的 feature 分支的历史更清晰易读，这是 `git merge` 做不到的。

### 黄金法则

> 永远不要对已经推到主干分支服务器或者团队其他成员的提交进行变基，我们选择变基还是合并的范围应该在自己当前工作范围内。

## 还原 git revert

> Revert some existing commits
>
> 恢复一些现有的提交

开发者可以使用 `git revert` 命令撤销已经 push 到远程仓库的 commit 提交。

当你想使用 `git reset` 或 `git rebase -i` 从历史记录中删除上一次 commit 提交，这也许不是一种很好的操作方式，主要是因为这会导致远程仓库与其他协作成员的本地仓库不同步。

![Revert](./snapshots/undoing_changes_001.png)

### 还原提交

`git revert` 会提交一个新的版本，将需要 revert 的版本的内容再反向修改回去，版本会递增，不影响之前提交的内容。

📖 **语法：**

```bash
# 还原前一次 commit 
$ git revert HEAD

# 还原前前一次 commit
$ git revert HEAD^

# 还原指定版本
$ git revert <commit-id>
```





---

**参考资料：**

* [📝 Git 由浅入深之细说变基](https://juejin.im/post/58f97793a22b9d00658b15b6)