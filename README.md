# Git和Github学习记录

Git是Linux发明者Linus开发的一款新时代的版本控制系统，应用广泛。<br/><br/>
![在这里插入图片描述](https://github.com/ChenYikunReal/git_learning/blob/master/images/Github.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg5NjMxOA==,size_16,color_FFFFFF,t_70)
<br/>
这里会试着分享一些自己学习和使用Git/Github/Gitee的一些经验！

## 项目目录
- books： 收集到的相关书籍资料
- exe： Git安装程序exe(Git在国内下载比较麻烦)
- images： Git学习期间的一些图片/截图

## Git与Github相连的基本配置
[真小白入门之Github](https://blog.csdn.net/nmjuzi/article/details/82184818)

## 提交到Github上的通常流程(个人简易版)
### 命令行提交
1. <code>git add .</code>
2. <code>git commit -m "注释"</code>
3. <code>git push -u origin master</code>

(严格讲，在push之前应该pull一下的：<code>git pull origin master</code>)
### IDEA(PyCharm、CLion、WebStorm等类似)提交
1. VCS → Import into Version Control → Create Git Repository...
2. 选定项目目录点右键 → Git
    1. Repository → Remotes... → + → 添加URL → ...
    2. Add
    2. Commit → Commit Message内容 → Commit按钮
    3. Repository → Push... → Push按钮

## Windows本地看不到.git
<code>.git</code>文件夹是隐藏文件夹，必须设置一下查看隐含文件夹才能看得到！

## DownloadZip和clone到的项目有何不同
据我所知，区别主要就是zip不含.git；而clone到的有.git。

## 解决本地历史和远端仓库历史不一致的方案
通常，比如我们新建一个Github的Repository(带README.md)，本地直接提交就会被<code>rejected</code>……<br/>
这种情况其实也蛮常见的，这就是我上面写到的“暴力提交三步走”的弊端——没考虑本地Git和远端Github的兼容。<br/>
此时，<code>git pull origin master --allow-unrelated-histories</code>命令就显得很香，可以解决此问题，基本屡试不爽！

## 修改Repository语言类型
我们可能会遇到一个非常恶心的问题：Repository语言不是我们期待的……<br/>
比如一个Vue/HTML项目，最后呈现出的是JavaScript；比如一个Java项目因为加了一些C工程代码而变成了CMake……<br/>
此时需要添加.gitattributes文件：<code>\*.\* linguist-language=java</code><br/>
上面的写法比较暴力，但确实挺实用的……

## 查看提交日志
<code>git log</code>这个命令我还是试过的，它能打印出你在这个Git目录下的commit记录。<br/>
如果是Git目录，则可以查看Log：<br/>
![在这里插入图片描述](https://github.com/ChenYikunReal/git_learning/blob/master/images/CorrectLog.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg5NjMxOA==,size_16,color_FFFFFF,t_70)
<br/>
如果不是Git目录，则不可以查看Log：<br/>
![在这里插入图片描述](https://github.com/ChenYikunReal/git_learning/blob/master/images/IncorrectLog.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg5NjMxOA==,size_16,color_FFFFFF,t_70)
<br/>
可以查看本地Git的记录

## 查看状态
查看当前仓库的状态可以用<code>git status</code>这个命令，有时候不知道进行到哪一步的话这个命令挺有用的。<br/>
大家可以在提交的每一步进行后使用这个命令看一看仓库状态。

## 分支与合并
说实话，直到写这部分，我还没怎么用过分支与合并，所有的项目基本都是自己来处理，那顺便学一下吧！<br/>
本地做个测试即可，首先需要创建一个本地仓库，我新建一个<code>git_test</code>文件夹，在此目录下使用<code>git init</code>命令可使git_test文件夹含.git文件夹。<br/>
注意此时新建项目的不能直接新建分支，否则会报错：<code>fatal: Not a valid object name: 'master'.</code>，也就是说必须先commit至少一次才行。<br/>
先随便放一个文件进去，再依次输入<code>git add .</code>、<code>git commit -m "测试"</code>，完成初次提交。<br/>
接下来新建分支<code>a_test</code>，命令为<code>git branch a_test</code>。<br/>
使用命令<code>git branch</code>可查看分支情况：
```text
  a_test
* master
```
<code>master</code>表示主干，<code>a_test</code>则是新建的分支，<code>\*</code>表示当前分支<br/>
使用命令<code>git checkout a_test</code>即可切换到a_test分支目录下：
```text
* a_test
  master
```
回到主分支，使用命令<code>git checkout -b b_test</code>可以新建b_test分支并切换过去：
```text
  a_test
* b_test
  master
```
接下来我们该测试合并分支了，切回master，使用命令<code>git merge a_test</code>，如果无冲突则直接合并成功：<br/>
```text
Already up to date.
```
删除分支要分情况：
- 分支与主干是同步的(如分支建错、已提交同步等)需要删除：<code>git branch -d a_test</code>
- 分支与主干是不同步的但需要强行删除：<code>git branch -D b_test</code>

## 给代码打上版本Tag
我们给初代版本打上Tag，表示版本为V1.0：<code>git tag v1.0</code>。
随着修改，我们提交后给新版本打上V1.1的Tag：<code>git tag v1.1</code>
如若需要回溯到V1.0版本，需使用命令<code>git checkout v1.0</code>：
```text
git checkout v1.0
Note: switching to 'v1.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 025ee41 测试

```
我们查看log，没有V1.1的log：
```text
git log
commit ......... (HEAD, tag: v1.0)
Author: ... <...@users.noreply.github.com>
Date:   Mon Jul 27 18:36:08 2020 +0800

    测试

```
然后我们返回至V1.1(<code>git checkout v1.1</code>)：
```text
git log
commit ......... (HEAD, tag: v1.1, master)
Author: ... <...@users.noreply.github.com>
Date:   Mon Jul 27 18:36:38 2020 +0800

    测试

commit ......... (tag: v1.0)
Author: ... <...@users.noreply.github.com>
Date:   Mon Jul 27 18:36:08 2020 +0800

    测试

```

## 查看当前项目所有的远程仓库
```text
git remote -v
```
如果没有远程仓库，则不会有输出。<br/>
如果有单一远程仓库，则显示如下内容：
```text
origin https://github.com/username/repository_name (fetch)
origin https://github.com/username/repository_name (push)
```

## 指定远程仓库的用户名和邮箱
比较一劳永逸的做法是指定全局的默认用户名和邮箱：
```text
git config --global user.name "username"
git config --global user.email "email_address"
```
我们也可以为专属的项目设置默认用户名和邮箱，只需去掉<code>--global</code>即可：
```text
git config user.name "username"
git config user.email "email_address"
```

## 配置相关
<code>git config -l</code>命令可查配置信息，这个配置文件的位置是Linux的<code>~/.gitconfig</code>

一些比较经典的配置如下(配置用户名、邮箱这种上面提过了)：
- <code>git config --global core.editor "vim" # 设置Editor使用vim</code>
- <code>git config --global color.ui true # 开启终端的各种颜色</code>
- <code>git config --global core.quotepath false #设置显示中文文件名</code>

## checkout再说明
checkout两个主要功能：
- 切换分支branch
- 切换版本标签tag

## 查看版本改动
查看版本改动需要使用<code>git diff</code>命令：
```text
git diff <$id1> <$id2> # 比较两次提交之间的差异
git diff <branch1>..<branch2> # 在两个分支之间比较
git diff --staged # 比较暂存区和版本库差异
```
直接输入git diff只能比较当前文件和暂存区文件(仍未执行git add的文件)差异。<br/>
- 查到的改动如果是 **红色的"-"** 则代表删除的内容。
- 查到的改动如果是 **绿色的"+"** 则代表增加的内容。

## stash的使用
<code>git stash</code>命令可以把当前分支所有没有commit的代码先暂存起来，此时使用<code>git status</code>查看仓库状态是很干净的。<br/>
而输入<code>git stash list</code>可查到一条暂存记录。<br/>
将暂存记录的还原到原分支中使用的命令是<code>git stash apply</code>，还原后使用命令<code>git stash drop</code>删掉这条暂存记录。<br/>
<code>git stash pop</code>会帮我们还原+删除stash记录。<br/>
另说，drop只删一条stash记录，而<code>git stash clear</code>删的是所有stash记录。

## 分支冲突
我们在开发的过程中一般都会约定尽量大家写的代码不要彼此影响，以减少出现冲突的可能，但是冲突总归无法避免的，我们需要了解并掌握解决冲突的方法。<br/>
冲突的地方由<code>====</code>分出了上下两个部分，上部分一个叫HEAD的字样代表是当前所在分支的代码，下半部分是另一个分支的代码。<br/>
对比很明显，所以我们很容易判断哪些代码该保留，哪些代码该删除。我们只需要移除掉那些老旧代码，而且同时也要把那些<code>\<\<\<HEAD</code>、<code>====</code>以及<code>\>\>\>\>\>\></code>这些标记符号也一并删除，最后进行一次commit就ok了。

## 新Repository的README.md
Github、Gitee这些仓库在New一个Repository时都会有新建README.md的选项，而README.md一般是不能缺的，所以开始的时候我都会直接顺手创建。<br/>
但是后来就出现了很多很多的麻烦，所以现在个人不建议直接创建README.md这东西，本地自己创一个文件写好内容直接push就行。

## 从Github撤销一次提交
```text
git reset --hard HEAD~
git push -f
```
