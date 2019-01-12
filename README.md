## git使用教程
[git教程地址](https://git-scm.com/book/zh/v2)
### git起步
#### git安装
- linux环境安装
```shell
  $ sudo yum install git
```
- mac环境安装
下载地址：http://git-scm.com/download/mac
#### 起步配置
Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：
- /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 --system 选项的 git config 时，它会从此文件读写配置变量。
- ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 可以传递 --global 选项让 Git 读写此文件。
- 当前使用仓库的 Git 目录中的 config 文件（就是 .git/config）：针对该仓库。

每一个级别覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。
在 Windows 系统中，Git 会查找 $HOME 目录下（一般情况下是 C:\Users\$USER）的 .gitconfig 文件。 Git 同样也会寻找 /etc/gitconfig 文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。
#### 用户信息
```shell
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
#### 检查配置信息
```shell
# 列出所有 Git 当时能找到的配置
git config --list
# 来检查 Git 的某一项配置
git config <key>
```
#### 获取帮助
```shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```
### git基础
#### 获取git仓库
> 有两种取得 Git 项目仓库的方法。 第一种是在现有项目或目录下导入所有文件到 Git 中； 第二种是从一个服务器克隆一个现有的 Git 仓库。
- 在现有目录中初始化仓库
```shell
$ git init
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```
- 克隆现有的仓库
```shell
# 这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。
$ git clone https://github.com/libgit2/libgit2
# 这将执行与上一个命令相同的操作，不过在本地创建的仓库名字变为 mylibgit。
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
#### 记录每次更新到仓库
![文件的状态变化周期](https://git-scm.com/book/en/v2/images/lifecycle.png)
```shell
# 检查当前文件状态
$ git status
# 跟踪新文件
$ git add README
# 状态简览
$ git status -s
# 忽略文件：一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

# 查看已暂存和未暂存的修改，要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 git diff，若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令
# 提交更新
$ git commit
# 跳过使用暂存区域提交：给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤
$ git commit -a -m 'added new benchmarks'
# 移除文件 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
$ git rm PROJECTS.md
# 另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。
$ git rm --cached README
$ git rm log/\*.log
# 移动文件
$ git mv file_from file_to
```
#### 查看提交历史
> 在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 git log 命令。

```
默认不用任何参数的话，git log 会按提交时间列出所有的更新，最近的更新排在最上面。 正如你所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。
```
```shell
# 一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交：
$ git log -p -2
# 如果你想看到每次提交的简略的统计信息，你可以使用 --stat 选项
$ git log --stat
# 另外一个常用的选项是 --pretty。 这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 oneline 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 short，full 和 fuller 可以用。
$ git log --pretty=oneline
# format，可以定制要显示的记录格式，具体选项参见：https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2
$ git log --pretty=format:"%h - %an, %ar : %s"
```
#### 撤销操作
```shell
# 取消暂存
$ git reset HEAD CONTRIBUTING.md
# 撤销
$ git checkout -- CONTRIBUTING.md
```
#### 远程仓库
```shell
# 查看远程仓库
$ git remote
# 你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。
$ git remote -v
# 添加远程仓库 shotname为简写
git remote add <shortname> <url>
# 拉取信息（不会自动合并）
$ git fetch [remote-name]
# 从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。
$ git pull
# 推送到远程仓库
$ git push origin master
# 查看远程仓库
$ git remote show [remote-name]
# 推送到远程
$ git push
# 拉取远程数据
$ git pull
# 重命名远程仓库名,想要将 pb 重命名为 paul，可以用 git remote rename 这样做：git remote rename pb paul
$ git remote rename
# 移除远程仓库
$ git remote rm paul
```
#### git 别名
```shell
# 创建别名
$ git config --global alias.co checkout
```
### git分支








