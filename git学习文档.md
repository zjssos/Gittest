**Git的基础知识**
=========
- Git是一个版本控制系统
版本控制用于记录一个或者是若干个文件内容的变化，以便将来查阅特定版本修订的情况,可以对不同时间点的项目的修改内容进行比较和操作。
- Git是分布式版本控制系统
![工作原理/流程 图片](https://note.youdao.com/yws/api/personal/file/2C0B0DAA4FE3485E97C70744D09B78FF?method=download&shareKey=59369fc0c58a2b06c71f34340cfcf64a)
Workspace:工作区
Index/Stage:暂存区
Repository:仓库区
Remote:远程仓库
基本流程如下
    1. 在工作区中修改文件
    2.将下次要提交的更改选择性地暂存
    3.提交更新，找到暂存区的文件，将快照永久性的添加到暂存区
快照： 在 Git 中，每当你提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照并保存这个快照的索引。 为了效率，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。

安装Git
======
[参照链接](https://blog.csdn.net/u011535541/article/details/83379151)

Git 基础操作
========
### 1.获取Git仓库
**将本地目录转换为Git仓库**
` $ git init`
该命令会创建一个.git的子目录，包含了初始化的Git仓库中所有的必须文件

**远程仓库的添加与使用**
1.克隆远程仓库
    `$ git clone <url>(https://github.com/libgit2/libgit2) <重命名>(可选)`
2.管理远程仓库
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
支持两种标签：轻量与附注
·轻量只是某个特定提交的引用
·附注是存储在Git数据库中的完整对象，可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。
