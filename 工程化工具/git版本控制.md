#一、git原理
##1. 简介
![](img/git.png)

- workspace工作区，通过git add * 命令将改动提交到暂存区，git pull 命令从远成仓库拉回数据
- index 暂存区，执行git commit -m '说明'命令就把改动提交到仓库区
- Repository 仓库区 本地仓库，git push origin master提交到远程仓库，git clone讲克隆远程仓库到本地
- remote 远程仓库
##2. 术语
- 仓库
- 分支
- 标记 某个分支某个时间点的状态
- 提交 提交代码后 仓库会创建 一个新的版本 后续被重新获得
- 修订revision 用来表示代码的一个版本状态

##3. 忽略特定文件.gitignore
	忽略某文件
	npm-debug.log
	忽略文件夹
	dist/
	node_modules/
	.idea/
##4. git忽略空的文件夹，若您想版本控制空文件夹则在空文件夹下放一个文件

##5.配置
	git config --list  显示当前配置
	git config -e [--global]  编辑git配置文件
	设置提交代码时的用户信息，是否加上全局--global自行决定，一般是直接设置全局的。另外用户邮箱需要注意最好使用gmail,QQ也可以，需要和你远程仓库保持一致不然你的contribution是不会被记录在远程仓库的
	git config [--global] user.name "[name]"
	git config [--global] user.email "[email address]"

#二、git
##1.首次推送及相关操作
<pre>
git add *
git commit -m ''
git remote add origin url
git push -u origin master
git add 文件 文件
git add 文件夹
git add *
git add -p 添加每个变换前都会确认，对同一文件的多处变化，可以实现分次提交
git rm file file 删除工作区文件，并将这次删除放在暂存区
git rm --cached file 停止追踪文件，文件保留在工作区
git mv 文件原名 文件新名  提交
git commit -a 提交工作区自上次commit之后的变化，直接到仓库区
git commit -v 提交时显示所有diff信息
git commit --amend -m message 使用一次行的commit覆盖上一次提交
git commit --amend file file  重做上一次的commit 并包括指定文件的新变化
git push origin master
git pull origin master
</pre>
##2.分支
<pre>

</pre>

##3.后悔药


* git checkout file 恢复暂存区的文件到工作区
* git checkout [commit] [file] 恢复某个commit的指定文件到暂存区和工作区
* git checkout . 恢复暂存区的所有文件到工作区
* git reset --hard HEAD^ 会退到上一个版本 head表示当前版本
* git reset file 重置暂存区的指定文件，与上一次commit保持一致，工作区不变
* git reset --hard 重置暂存区和工作区，与上一次commit保持一致
* git reset [commit] 重置打那个钱分支的指针为指定commit，
* git reset --hard [commit]
* git reset --keep [commit]
* git revert [commit]
* git stash 
* git stash pop、

### 版本回退

git reset --hard HEAD^ 一个代表回退到上一个版本，HEAD^^^三个以前的

回退到之前的版本，又想再返回去，可是不知道版本号了

可通过git reflog用来记录git的每一次命令，可查看版本号，由此确定回到未来哪个版本

git log可查看提交历史，以便确定要回退到哪个版本



##4.标签

* git tag
* git tag [tag]
* git tag [tag] [commit]
* git tag -d [tag]
* git push origin :ref/tags/[tagName]
* git show [tag]
* git push [remote] [tag]
* git push [remote] --tags
* git checkout -b [branch] [tag]

##5.分支
* git branch 列出所有本地分支
* git branch -r 远程分支
* git branch -a 所有本地和远程
* git branch [branch-name] 新建分支，不切换
* git branch -d hotfix 删除分支
* git checkout -b [branch] 新建一个分支，并切换到该分支
* git branch [branch] [commit] 新建一个分支。指向指定commit
* git branch --track [branch] [remote=branch]
* git checkout [branch-name] 切换到这个分支
* git checkout -
* git branch --set-upstream [branch] [remote-branch]
* git merge [branch]
* git push origin --delete [branch-name] 删除远程分支
* git branch -dr [remote/branch ]

切换分支时最好保持一个清洁的工作区域，

git merge





[原文链接](http://igit.linuxtoy.org/getting-old-versions.html)

## 设置

设置git 姓名和email

```
git config —global user.name 'yourname'
git config --global user.email 'youremail'
```

## 检查状态

```
git status
```

查看当前仓库的状态

## 暂存更改

```
git add .
```

暂存更改，但是并没有将这些更改永久的放在仓库中没有commit

## 暂存和提交

```
git add a.rb
$ git add b.rb
$ git commit -m "Changes for a and b"

$ git add c.rb
$ git commit -m "Unrelated change to c"
```

分开暂存和提交，你能够更加容易地调优每一个提交。

## 日志log

```
$ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short

$ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
* 1f7ec5e 2013-04-13 | Added a comment (HEAD, master) [Jim Weirich]
* 582495a 2013-04-13 | Added a default value [Jim Weirich]
* 323e28d 2013-04-13 | Using ARGV [Jim Weirich]
* 9416416 2013-04-13 | First Commit [Jim Weirich]
```

- `--pretty="..."` 定义输出的格式
- `%h` 是提交 hash 的缩写
- `%d` 是提交的装饰（如分支头或标签）
- `%ad` 是创作日期
- `%s` 是注释
- `%an` 是作者姓名
- `--graph` 使用 ASCII 图形布局显示提交树
- `--date=short` 保留日期格式更好且更短

## 获取旧版本

```
git log
获取hash值
git checkout <hash>切换到此状态
```

git log 



## 打标签

```
git tag v1 给当前状态打标签
git checkout v1^ 切换到上一个版本
git tag v1-beta 给当前状态打个标签
git checkout v1 可以使用checkout切换
git checkout v1-beta
```



## 撤销本地更改

```
git checkout hello.js
```

检测出文件在**仓库中的最新版本**与本地文件统一



## 撤销暂存的更改

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   modified:   hello.rb
#

$ git reset HEAD hello.rb
Unstaged changes after reset:
M   hello.rb
```

重置head中暂存区的内容，清除我们已经暂存的更改

reset命令不会更改工作目录，只修改了暂存区的内容，还需使用git checkout hello.rd更改



## 撤销提交的更改

```
git revert HEAD

$ git hist
* a10293f 2013-04-13 | Revert "Oops, we didn't want this commit" (HEAD, master) [Jim Weirich]
* 838742c 2013-04-13 | Oops, we didn't want this commit [Jim Weirich]
* 1f7ec5e 2013-04-13 | Added a comment (v1) [Jim Weirich]
* 582495a 2013-04-13 | Added a default value (v1-beta) [Jim Weirich]
* 323e28d 2013-04-13 | Using ARGV [Jim Weirich]
* 9416416 2013-04-13 | First Commit [Jim Weirich]
```

撤销的提交其实是**创建一个提交**



## 分支中移除提交

使用revert  在log中依然可见，若不想则应该使用

```
git reset --hard 
```

- --soft – 缓存区和工作目录都不会被改变
- --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响    
- --hard – 缓存区和工作目录都同步到你指定的提交


- `git reset --mixed（可删除默认） HEAD` 将你当前的改动从缓存区中移除，但是这些改动还留在工作目录中。
- 如果你想完全舍弃你没有提交的改动，你可以使用`git reset --hard HEAD`。

**已经commit并且push到远程仓库的不允许reset**

## git checkout和git reset

- 文件层面 

```
git checkout file  将文件回退到仓库commit中的最新文件
git reset HEAD file  将暂存区的文件撤销
git reset eb43bf file.txt 将文件回退到某个版本
```

- 非文件

```
git checkout hash  会切换分支，换head，更改暂存区内容，切换工作区内容
git reset hash
```

git checkout 很像git reset —hard 但是又区别：

- reset会把working directory里的所有内容都更新掉，checkout不会去修改你在Working Directory里修改过的文件
- reset把branch移动到HEAD指向的地方，checkout则把HEAD移动到另一个分支

![](http://img0.tuicool.com/yYFf6vV.png!web)

## 修正提交

```
$ git add hello.rb
$ git commit -m "Add an author comment"

$ git add hello.rb
$ git commit --amend -m "Add an author/email comment"

可以看到没有add an author comment
$ git hist
* eb30103 2013-04-13 | Add an author/email comment (HEAD, master) [Jim Weirich]
* 1f7ec5e 2013-04-13 | Added a comment (v1) [Jim Weirich]
* 582495a 2013-04-13 | Added a default value (v1-beta) [Jim Weirich]
* 323e28d 2013-04-13 | Using ARGV [Jim Weirich]
* 9416416 2013-04-13 | First Commit [Jim Weirich]
```

## 解决冲突

```
$ git checkout greet
Switched to branch 'greet'
$ git merge master
Auto-merging lib/hello.rb
CONFLICT (content): Merge conflict in lib/hello.rb
Automatic merge failed; fix conflicts and then commit the result.

如果你打开 lib/hello.rb，那么你将看到：

<<<<<<< HEAD
require 'greeter'

# Default is World
name = ARGV.first || "World"

greeter = Greeter.new(name)
puts greeter.greet
=======
# Default is World

puts "What's your name"
my_name = gets.strip

puts "Hello, #{my_name}!"
>>>>>>> master

第一部分是当前分支（greet）头的版本。第二部分是 master 分支 的版本

require 'greeter'
puts "What's your name"
my_name = gets.strip
greeter = Greeter.new(my_name)
puts greeter.greet

重新提交一遍
$ git add lib/hello.rb
$ git commit -m "Merged master fixed conflict."
Recorded resolution for 'lib/hello.rb'.
[greet 25f0e8c] Merged master fixed conflict.
```

## 远程仓库

```
$ git remote show origin
* remote origin
  Fetch URL: /Users/jim/working/git/git_immersion/auto/hello
  Push  URL: /Users/jim/working/git/git_immersion/auto/hello
  HEAD branch (remote HEAD is ambiguous, may be one of the following):
    greet
    master
  Remote branches:
    greet  tracked
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

## 取得远程仓库的更新

`git fetch` 命令的结果将从远程仓库取得新的提交，但它 不会将这些提交合并到本地分支中

```
git fetch 
```

## 将拉下的更新合并到本地

```
git merge origin/master
```

git pull



## 添加跟踪远程分支的本地分支

```
$ git branch --track greet origin/greet
Branch greet set up to track remote branch greet from origin.
$ git branch -a
  greet
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/greet
  remotes/origin/master
$ git hist --max-count=2
* 2e4c559 2013-04-13 | Changed README in original repo (HEAD, origin/master, origin/HEAD, master) [Jim Weirich]
* 2fae0b2 2013-04-13 | Updated Rakefile (origin/greet, greet) [Jim Weirich]
```



git rebase？？