
# git 一些经验

## 入门

  初始化一个Git仓库，使用git init命令。
  添加文件到Git仓库，分两步：
  第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
  第二步，使用命令git commit，完成。

## 查看变化
```
  gitt status 
```
  如果 git status 告诉你有文件被修改过，用 
```
git diff 
```
  可以查看修改内容。Git鼓励大量使用分支：

## 时光穿梭

  HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
  穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
  要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

## 基本概念
  Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
  git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支

## diff
    每次修改，如果不add到暂存区，那就不会加入到commit中
    提交后，用
```
git diff HEAD -- readme.txt
```
    命令可以查看工作区和版本库里面最新版本的区别

## 切换分支 
```
    git checkout -- readme.txt
```
    意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

    一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

    一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

    总之，就是让这个文件回到最近一次git commit或git add时的状态。

    ----------------------------------------------------------------------------------------------
    场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

    场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

##  reset
    Git同样告诉我们，用命令
```
git reset HEAD file
```
    可以把暂存区的修改撤销掉（unstage），重新放回工作区：
    git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

## rm 

    命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## remote

    要关联一个远程库，使用命令
```
    git remote add origin git@server-name:path/repo-name.git；
```

    关联后，使用命令
```
    git push -u origin master
```
    第一次推送master分支的所有内容；

    此后，每次本地提交后，只要有必要，就可以使用命令
```
    git push origin master
```
    推送最新修改；

    分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

## clone

    要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

    Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

## ...

    查看分支：git branch

    创建分支：git branch <name>

    切换分支：git checkout <name>

    创建+切换分支：git checkout -b <name>

    合并某分支到当前分支：git merge <name>

    删除分支：git branch -d <name>

## 解决冲突

    当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

    用
```
    git log --graph
```
    命令可以看到分支合并图。

## tip
    Git分支十分强大，在团队开发中应该充分应用。

    合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
## stash pop

    修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

    当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，
```
    git stash pop
```
    回到工作现场。

##  ---

    开发一个新feature，最好新建一个分支；

    如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

## 多人合作

    查看远程库信息，使用git remote -v；

    本地新建的分支如果不推送到远程，对其他人就是不可见的；

    从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

    在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

    建立本地分支和远程分支的关联，使用
```
    git branch --set-upstream branch-name origin/branch-name；
```

    从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

## 标签

    命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
```
    git tag -a <tagname> -m "blablabla..."
```
    可以指定标签信息；

```
    git tag -s <tagname> -m "blablabla..."
```
    可以用PGP签名标签；

    命令git tag可以查看所有标签。

## 操作标签

    命令git push origin <tagname>可以推送一个本地标签；

    命令git push origin --tags可以推送全部未推送过的本地标签；

    命令git tag -d <tagname>可以删除一个本地标签；

    命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

## github

    在GitHub上，可以任意Fork开源仓库；

    自己拥有Fork后的仓库的读写权限；

    可以推送pull request给官方仓库来贡献代码。

## 搭建 Git 服务器
    要方便管理公钥，用 Gitosis；

    要像SVN那样变态地控制权限，用Gitolite。


## 跟踪一个文件
- git blame file 和 git log file 
会有不同。 同样是跟踪一个文件。
```
git blame file 
```
    输入每一行最后修改的人，

```
git log file 
```
    输出文件最后提交的人

--------------------------------------------------------------------------------------------------
## git 忽略文件权限修改
-	所有git库:   
```
git config --global core.fileMode false
```
当前库:  
```
    git config core.fileMode false
```
## 循环切换
```
git checkout - 
```
和
```
cd - 
```
类似 , 会一直在上一个分支和现在的分支之间一直切换, cd - 会一直和上一个目录切换。

-------------------------------------------------------------------------------------------------
## 忽略用户名和密码push
- ~ 目录下面，
- touch .git-credentials 新建一个文件
- vim .git-credentials
- 添加 https://username:password@github.com
- 终端配置git config --global credentials.helper store


-------------------------------------------------------------------------------------------------
```
git fsck --lost-found 
git show hashidxxxxxx
git rebase hashidxxxxx 
```
[参考](http://gitbook.liuhui998.com/5_9.html )

## 分支重命名
```
git branch -m old-branch-name  new-branch-name

git rebase master -i 
```
  出现多个分支的提交, 可以将那些需要合并到一个提交的commit前面的pick加改成s, 即meld的选择项, 可以在rebase过程中合并开发的过程到一个commit中, so sexy! 

## 批量删除本地已经合并到master的分支: 
```
git branch --merged | grep -v "\*" | grep -v master | grep -v dev | xargs -n 1 git branch -d

git fetch --prune && git branch -r | awk '{print $1}' | egrep -v -f /dev/fd/0 <(git branch -vv | grep origin) | awk '{print $1}' | xargs git branch -d

```
## 删除远程文件目录 
```
git rm -r --cached dirname
```
比如 node_module, logs 之类无需上传的目录
