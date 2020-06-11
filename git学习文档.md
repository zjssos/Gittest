**Git的基础知识**
=========
**Git如何存储文件**
- 这东西放在开头是为了能更好理解Git的工作流程
- 核心是存储键值对，它允许插入任意类型的内容，并会返回一个键值，通过该键值可以在任何时候再取出该内容。
- Git数据对象(blob)
Git 存储数据内容的方式，为每份内容生成一个文件，取得该内容与头信息的 SHA-1 校验和，创建以该校验和前两个字符为名称的子目录，并以 (校验和) 剩下 38 个字符为文件命名 (保存至子目录下)。
-  Tree对象
解决文件名的保存问题，允许我们将多个文件组织到一起。一个树对象包含了一条或多条树对象记录（tree entry），每条记录含有一个指向数据对象或者子树对象的 SHA-1 指针，以及相应的模式、类型、文件名信息。
- Commit对象
commit 对象有格式很简单：指明了该时间点项目快照的顶层树对象、作者/提交者信息（从 Git 设置的 user.name和user.email中获得)以及当前时间戳、一个空行，以及提交注释信息。每一个commit对象都指向了你创建的树对象快照。

**Git工作流程**
- Git是一个版本控制系统
版本控制用于记录一个或者是若干个文件内容的变化，以便将来查阅特定版本修订的情况,可以对不同时间点的项目的修改内容进行比较和操作。
作为一个文件管理系统
- Git是分布式版本控制系统
![工作原理/流程 图片](https://img.mukewang.com/59c31e4400013bc911720340.png)
Workspace:工作区
Index/Stage:暂存区
Repository:仓库区
Remote:远程仓库
基本流程如下  
    1. 在工作区中修改文件
    2.将下次要提交的更改选择性地暂存
    3.提交更新，找到暂存区的文件，将快照永久性的添加到Git目录
现在来看解释快照、add、commit之间的联系，以此来解释它们的原理
**add**会为每个文件计算校验和(通过SHA-1 哈希算法)，然后会把当前版本的文件快照保存到Git仓库中，最后将校验和加入暂存区等待提交。
**commit** 进行commit操作时Git会计算每一个子目录的校验和，然后在Git仓库中将这些校验和保存为树对象，随后，Git会创建一个提交对象，它包含了以上提到的信息和指向树对象的指针，因此在需要的时候，Git就能重新此次保存的快照。
**快照** 系统某个时刻的状态记录。 为了效率，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。快照保存的应该就是Index中状态。
下图表示在工作目录里，存在着三个将被暂存(add)和提交(commit)的文件
![ ](https://git-scm.com/book/en/v2/images/commit-and-tree.png)

**Git分支介绍**
- 分支就是把工作从主线上分离开，避免影响主线的开发。
- Git分支的本质就是一个指向提交对象的可变指针
- 对文件做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象（父对象）的指针。
![ ](https://git-scm.com/book/en/v2/images/commits-and-parents.png)
- 在分支上的每一次提交会使得分支自动前移，因此可以在当前对象创建新分支并选择该新分支来进行不同方向的开发。
- 分支可以合并(merge)和变基(rbase)来达到分模块的开发、也可基于此来进行Bug修复
- 整个过程看上去就像链表的增删改查

安装Git
======
[参照链接](https://blog.csdn.net/u011535541/article/details/83379151)

Git 基础操作
========
######< >表示变量
### 1.获取Git仓库
**将本地目录转换为Git仓库**
` $ git init`
该命令会创建一个.git的子目录，包含了初始化的Git仓库中所有的必须文件

**远程仓库的添加与使用**
- 1.克隆远程仓库
    `$ git clone <url>(https://github.com/libgit2/libgit2) <重命名>(可选)`
- 2.管理远程仓库
· Git 保存当前仓库的简写与其对应的URL
`git remote <缩写>`
· 添加远程仓库
`git remote add <简写> <url>`
· 重命名与移除
`git remote rename <name> <rename>`
`git remote rm <name>`
· 查看远程仓库
`git remote -v`
· 远程仓库的抓取与拉取
`git fetch <>`只从仓库抓取本地仓库没有的数据，就是在工作区添加了文件，并没有加入Git
`git pull <>`算是两个命令的合集，它会自动尝试将抓取的数据合并到当前所在分支
· 推送
`git push <remote> <branch>`
推送branch分支到romatef仓库，必需是有写如权限和别人没有推送过，如果推送过的话的抓取他人的工作并合并后才行 
### 2.添加与提交
###### 工作目录下的文件分为已跟踪或未跟踪
**跟踪新文件**
    
    $ git add git学习文档.md

**状态预览**
    `$ git status`
可以看到   
    ```
    On branch master   
    No commits yet
    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
        new file:   "git\345\255\246\344\271\240\346\226\207\346\241\243.md"```
只要在 Changes to be committed 这行下面的，就说明是已暂存状态。这样的话这个add命令应该理解为精确地将内容添加到下一次提交中
对文件进行修改后再进行状态预览，会发现文件会同时出现在暂存区和非暂存区

    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
        new file:   "git\345\255\246\344\271\240\346\226\207\346\241\243.md"

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified:   "git\345\255\246\344\271\240\346\226\207\346\241\243.md"
如果要提交修订后的文件需要重新运行 `git add`
**查看文件差异**
`git diff` 有可视化工具
**提交更新**
`git commit`
Git的编辑器会跳出一个文本,在里面可以编写提交的说明
也可直接 `git commit -m 'This is third commit'`
```
 $ git commit
hint: Waiting for your editor to close the file...
[master (root-commit) 553d181] This is third commit
 1 file changed, 10 insertions(+)
 create mode 100644 "git\345\255\246\344\271\240\346\226\207\346\241\243.md"
```
跳过使用暂存区(add),将跟踪过的文件暂存一并提交`git commit -a `
**移除文件**
必须从跟踪文件清单也就是暂存区中移除，然后进行提交。
使用`-f`是强制删除，git无法恢复数据
```
$ git rm -f temp.txt
```
使用`--cached`代表从暂存区移除，但保留在工作目录中
`$ git rm --cached temp.txt
`
**移动文件**
git中并显示的跟踪文件移动操作。
`git mv temp.txt 1`
相当于
```
$ mv temp.txt 1
$ git rm temp.txt
$ git add 1
```

### 3.查看提交历史
`git log `

|  常用参数 |  作用  |
|---|----|
| `-p` | 显示每次提交所引入的差异 |
| `-N` | N为显示最近N次的提交 |
|`--stat`| 每次提交的简略统计信息|
|`-- pretty=oneline\short\full\fuller`|可以使用不同于默认格式的方式展示提交历史|
还有其它的参数，但是基本作用都是用来做格式控制的，但本质上还是对提交历史的一个显示。
### 4.标签
支持两种标签：轻量与附注,标签和分支相似，都是指向提交对象的
- 轻量只是某个特定提交的引用
`git tag <ver>`
- 附注是存储在Git数据库中的完整对象，可以被校验的，其中包含打标签者的名字 、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。
`git tag -a <ver> -m "标签信息"`
查看标签信息 
`git show <ver>`
后期打标签
`git tag -a <ver> SHA-1校验和`
共享标签
`git push <remote> <ver>/--tags`
删除标签
本地仓库`git tag -d <ver>`
远程`git push <remote> --delete<tagname>`
### 5.分支的使用
- **新建与合并**
Git默认分支是master，如果在master上做过提交的话，
![ ](https://git-scm.com/book/en/v2/images/basic-branching-1.png)
创建分支并切换到该分支
`git checkout -b iss53`<==>`git branch iss53 + git checkout iss53`
然后在该分支上提交
![](https://git-scm.com/book/en/v2/images/basic-branching-3.png)
**注意**如果该分支任务结束或者要切回master或其他分支上继续开发的话要注意暂存区和工作目录那些没有被提交的修改，它可能会和即将检出的分支产生冲突阻止分支切换。可以使用暂存（stashing） 和 修补提交（commit amending）来绕过这个问题。
