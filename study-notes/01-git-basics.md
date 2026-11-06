# Git 基础练习笔记

这次练习中，我在 `mall` 项目里创建了 `practice/git-basics` 分支，写了一份笔记，提交后推送到 GitHub。

## 1. 从修改到推送

| 命令 | 作用 |
|---|---|
| `git status` | 查看当前分支，以及哪些文件已修改、已暂存或尚未跟踪 |
| `git switch -c practice/git-basics` | 从当前位置创建并切换到练习分支 |
| `git diff` | 查看已跟踪文件在工作区和暂存区之间的差异 |
| `git add study-notes/01-git-basics.md` | 把笔记此刻的内容放入暂存区 |
| `git diff --staged` | 检查下一次提交将包含哪些修改 |
| `git commit -m "docs: 添加 Git 学习笔记"` | 把暂存区的内容保存为一次本地提交 |
| `git show --stat` | 查看最新提交的说明、修改的文件和增删行数 |
| `git push -u origin practice/git-basics` | 推送到远程同名分支，并建立跟踪关系 |

## 2. 提交说明怎么写

常用格式是 `类型(范围): 具体改动`，范围可以省略。

- `docs: 添加 Git 学习笔记`（文档）
- `fix(admin): 修复 Swagger 文档访问问题`（修复）
- `feat(product): 支持按价格区间筛选商品`（新功能）
- 其他常见类型有 `refactor`（重构）、`test`（测试）、`perf`（性能优化）、`chore`（维护）。

## 3. HEAD 和本地分支

平时在分支上开发时，关系是 `HEAD → 当前本地分支 → 一条提交`。`git show` 默认展示 `HEAD` 指向的提交。

通常只有一个正在编辑的 `mall` 目录，多个分支共用 `.git` 里的提交历史。执行 `git switch study/setup` 会切回配置分支，工作目录随之更新。只在练习分支提交过的笔记会消失，切回来又会出现。未提交的改动可能跟着切换，所以切换前先看 `git status`。

## 4. origin 和远程分支

`origin` 是远程仓库的简称，`practice/git-basics` 是分支名。第一次推送时，Git 可以自动在远程创建同名分支。`-u` 建立跟踪关系，以后在这个分支上执行 `git push` 即可继续推送。

用 `git branch -vv` 查看跟踪关系。如果本地分支旁显示 `[origin/practice/git-basics]`，说明它跟踪的是 `origin` 仓库里的练习分支。

## 5. 查看差异与撤销修改

`diff` 用来比较两个版本，`restore` 用一个版本恢复另一个位置的文件。`HEAD` 表示当前提交。

| 命令 | 作用 |
|---|---|
| `git diff` | 比较暂存区和工作区，查看已跟踪文件中尚未暂存的修改 |
| `git diff --staged` | 比较 `HEAD` 和暂存区，查看下次提交会包含的修改 |
| `git restore 文件名` | 用暂存区版本恢复工作区文件，丢弃该文件尚未暂存的修改 |
| `git restore --staged 文件名` | 用 `HEAD` 版本恢复暂存区，撤销该文件的暂存，保留工作区内容 |

例如，笔记提交时是 A，改成 B 后执行了 `git add`，随后又改成 C。
此时按「当前提交、暂存区、工作区」排列是 `A、B、C`。从这个起点任选一条执行：

- `git restore 文件名` → `A、B、B`：工作区回到 B。
- `git restore --staged 文件名` → `A、A、C`：撤销暂存，仍可继续编辑 C。