[TOC]

# Git的使用方式

- 命令行

- 图形化界面（GUI）

- IDE插件/扩展

  

# 配置仓库

指令为：

```
git config --"参数" 用户名(或用户邮箱)
```

参数为：

省略（Local):本地配置，只对本地仓库有效

--global : 全局配置，对所有仓库有效

--system : 系统配置，对所有用户有效

还可以用下面的命令来保存用户名和邮箱：

```
git config --global credential.helper store
```

然后可以用下面的命令来查看配置的用户名和邮箱：

```
git config --global --list
```

## 创建一个版本库

先创建目录：

```
mkdir learngit
cd learngit
pwd //显示当先目录
```

再把这个目录变为git可以管理的仓库：

```
git init
```

## 将文件添加到仓库

用`git add`将文件添加近仓库：

```
git add 所要添加的文件
```

注意：文件不止输入文件名还用输入文件格式。比如将`readme.txt`文件添加进仓库就怎么用

```
git add readme.txt
```

## 将文件提交到仓库

```
git commit -m "此次提交的说明"
```

`git commint`命令执行成功后会告诉：`1 file changed`：1个文件被改动（我们新添加的`readme.txt`文件）；`2 insertions`：插入了两行内容（`readme.txt`有两行内容）。

# 时光机

## 查看仓库当前状态

用`git staus`命令可以查看是否有被修改但没有被提交(与在仓库的原文件进行比较)的文件。

## 查看修改内容

用`git diff`可以看修改过的`readme.txt`与之前提交进仓库里的`readme.txt`有什么修改。

注意：此时存在于文件夹里`readme.txt`以被修改，但没有提交进仓库

## 将修改后的文件提交

像第一次添加和提交一样用`git add`和`git commit -m`

注意：要在`git commit -m`后写清楚修改的描写或版本特征，方便回溯。

## 查看仓库版本

用`git log`命令查看，出现以下内容：

```
commit 5a3a9adb74f59051efb775f8314c071694d337ba (HEAD -> master)
Author: ctyhail <2024735059@qq.com>
Date:   Wed Dec 13 22:16:33 2023 +0800

    my exspression

commit b855b23f59142bf5350b7262c82a782b1fbf1992
Author: ctyhail <2024735059@qq.com>
Date:   Wed Dec 13 22:12:27 2023 +0800

    add distribute

commit b782ba77b32bdfb879edb82fdbd095b5433512a7
Author: ctyhail <2024735059@qq.com>
Date:   Wed Dec 13 22:03:37 2023 +0800

    worte a readme file

```

其中每一个单独成行的文字就是每一个版本的描述。

如果嫌信息过多可以加上`--pretty=oneline`参数，界面会变得：

```
5a3a9adb74f59051efb775f8314c071694d337ba (HEAD -> master) my exspression
b855b23f59142bf5350b7262c82a782b1fbf1992 add distribute
b782ba77b32bdfb879edb82fdbd095b5433512a7 worte a readme file
```

需要友情提示的是，你看到的一大串类似`1094adb...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

## 回退仓库版本

在git中用`HEAD`来表示当前版本，也就是最新的提交，上个版本就是`HEAD^`，上上个版本就是`HEAD^^`，以此推类。但一般`^`写太多数不过来，可以用简写成`HEAD~n`，其中n是往回退的版本数。

回退版本的命令为`git reset`，使用方法为：

```
git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

这是把当前版本`append GPL`回退到上一个版本`add distributed`。

*可以使用`cat+文件`来查看文件内容：

```
cat readme.txt
this is my first repository
a good beginning is half down
nothong is impossible
```

此时用`git log`查看版本状况为：

```
commit b855b23f59142bf5350b7262c82a782b1fbf1992 (HEAD -> master)
Author: ctyhail <2024735059@qq.com>
Date:   Wed Dec 13 22:12:27 2023 +0800

    add distribute

commit b782ba77b32bdfb879edb82fdbd095b5433512a7
Author: ctyhail <2024735059@qq.com>
Date:   Wed Dec 13 22:03:37 2023 +0800

    worte a readme file

```

最新的版本已经不见了，如何想会到最新的版本需要寻找最新版本`append GPL`的`commit id`。在上面，我们可以找到其`commit id`为`5a3a9adb74f59051efb775f8314c071694d337ba`。版本号没必要写全，可以写出前几位让git自己去寻找。回退到最新版本：

```
git reset --hard 5a3a9
HEAD is now at 5a3a9ad my exspression
```

如果命令窗口关闭导致找不到目标版本的`commit id`。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```
git reflog
5a3a9ad (HEAD -> master) HEAD@{0}: reset: moving to 5a3a9
b855b23 HEAD@{1}: reset: moving to HEAD^
5a3a9ad (HEAD -> master) HEAD@{2}: reset: moving to 5a3a
b782ba7 HEAD@{3}: reset: moving to HEAD~2
5a3a9ad (HEAD -> master) HEAD@{4}: commit: my exspression
b855b23 HEAD@{5}: commit: add distribute
b782ba7 HEAD@{6}: commit (initial): worte a readme file
```

这样就可以找到最新版本的的`commit id`。

# 工作区域暂存区

## 工作区

就是能在电脑能看得见的目录

![image-20240109122010490](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240109122010490.png)

版本