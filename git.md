# git全知基础教程
链接：https://www.liaoxuefeng.com/wiki/896043488029600
### 创建版本库
##### git init 将普通目录变成Git可以管理的仓库（仅该命令可以在普通目录下执行）
当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，如果你没有看到.git目录，那是因为这个目录默认是隐藏的
##### git add 把文件添加到缓存区
##### git commit -m "your message" 把文件提交到仓库
##### git status 查看仓库状态
on branch master 提示你在master branch
changes not staged for commit: 没有add但修改过的文件
changes to be commited: 已经add的文件
untracked files: 新添加且未add的文件
nothing to commit, working tree clean 工作区没有任何修改
### 时光机穿梭
git diff readme.txt 查看本机与仓库中readme.txt文件
#### 版本回退
##### git log 告诉我们提交历史
一大串类似1094abd...是commit id(版本号)
##### git reset --hard HEAD^ 回退到上一版本
HEAD^^ 上上一个版本
HEAD是一个指向不同版本号的指针
HEAD^...还可以用commit id代替，不用输完整的commit id，git会自动补齐
##### git reflog 查看命令历史
用来回到未来版本
#### 工作区和暂存区
##### 工作区 working directory
电脑中能看到的目录
##### 版本库 repository
即隐藏目录.git，保存了很重要的暂存区(stage或者index)
分支和指针HEAD也在其中
#### 管理修改
##### git管理的是修改，而不是文件
##### git diff HEAD -- readme.txt 命令可以查看工作区和版本库里面最新版本的区别
#### 撤销修改
##### git checkout --file 让工作区file回到最近一次git commit或git add时的状态
##### 命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
#### 删除文件
##### rm file 在工作区删除文件
若是误删，可以用git checkout --file还原
##### git rm file 类似于git add file，之后仍需commit
### 远程仓库
SSH key的设立，获得修改远程仓库的权限，目测与本次课程无关
#### 添加远程库
##### git remote add origin git@github.com:michaelliao/learngit.git
把上面的michaelliao替换成你自己的GitHub账户名，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库
##### git push -u origin master
用git push命令，实际上是把当前分支master推送到远程，由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
##### git push origin master 之后把本地master分支的最新修改推送至GitHub
##### SSH警告 目测无关
#### 从远程库克隆
##### git clone git@github.com:michaelliao/gitskills.git
也可以用https://github.com/michaelliao/gitskills.git
### 分支管理
#### 创建与合并分支
HEAD严格来说不是指向提交，而是指向master，master才是指向提交的
##### git branch dev 创建dev分支
##### git checkout dev 切换到dev分支
##### git checkout -b dev 创建并切换到dev分支
##### git branch 查看当前所有分支
当前分支前标*号
##### git merge dev 
git merge命令用于合并指定分支到当前分支
Fast-forward表示这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快
##### git branch -d dev 删除dev分支
##### git switch -c dev 创建并切换到新的dev分支
##### git switch master 直接切换到已有的master分支
#### 解决冲突
Your branch is ahead of 'origin/master' by 1 commit.  Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
##### git merge失败后，git会自动修改冲突文件，修改文件即可
#### 分支管理策略
##### git merge --no-ff -m "merge with no-ff" dev 禁用Fast forward
避免在删除分支后丢掉分支信息
#### Bug分支
需要先隐藏当前工作区？
##### git stash 隐藏当前工作区
##### git stash list 查看隐藏工作区
##### git stash apply
恢复后还需用git stash drop删除
##### git stash pop
恢复后自动删除
##### git stash list 查看stash内容
##### git stash apply stash@{0} 指定恢复内容
##### git cherry-pick 4c805e2 复制一个特定的提交到当前分支
##### git branch -D feature-vulcan
在未合并情况下强行删除分支
#### 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin
##### git remote
查看远程库信息
##### git remote -v
查看更详细信息
##### 推送分支
推送时要指定本地分支 git push origin master
并不需要把所有的本地分支往远程推送
##### 抓取分支
（可以在本地merge branch,也可以直接传到远程再处理）
###### git branch --set-upstream-to=origin/dev dev
指定本地dev分支与远程origin/dev分支的链接，如果远端dev与本地dev有冲突，正常处理即可
1. 首先，可以试图用git push origin <branch-name\>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name\>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name\> origin/<branch-name>。
### 标签管理
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起
#### 创建标签
##### git tag v1.0 切换到打标签的分支上后再输入
##### git tag 查看所有标签,按字母排序
##### git show \<tagname\> 查看标签信息
##### git tag v0.9 f52c633 给某个id的commit打标签
##### git tag -a v0.1 -m "version 0.1 released" 1094adb  用-a指定标签名，-m指定说明文字
#### 删除标签
##### git tag -d v0.1 删除本地标签
##### git push origin <tagname>
##### git push origin --tags 推送所有标签到远程
##### git push origin :refs/tags/v0.9 从远程删除

## git工作流程（暂不使用）
链接：https://www.youtube.com/watch?v=8UguQzmswC4
### How to Use the Github Workflow
#### key Terms
certral repository 属于公司或集体
fork 把前者复制到自己的github账户上
clone 把前两者复制到电脑上
#### 工作步骤
1. organization上建一个repository
2. fork到自己的Github上
3. clone到local machine
4. 新建feature branch
5. 进行add,commit,push(git commit -a -m "message" 简化指令) git push \<remotename\> \<branchname\>
6. 回到github，可以开启一个pull request,将自己的feature branch上的修改提交到central repository的master branch
7. 删除feature branch
#### 将master branch of central repository同步到clone of your own fork
git remote add upstream<name> <adress of central repostiry>
git fetch upstream
git merge --ff-only upstream/master

## commit备注规范
链接：https://www.conventionalcommits.org/en/v1.0.0/
\<type\>[(optional scope)]: \<description\>

[optional body]

[optional footer(s)]

#### message header
1. type是必须的，其余都是可选的
2. scope必须在括号里
3. description必须紧跟在冒号和一个空格之后，是对代码改变的简单总结
4. commit body形式比较自由，可以有多段落，但必须另起一行
5. 每个footer必须包含一个word token，word token内用 - 代替空格，比如asked-by,除了BREAKING CHANGE
6. BREAKING CHANGE必须在前文中有所体现，必须用大写，可以在type末尾加!代替BREAKING CHANGE
7. 一个commit只能有一个type，否则就要重新组织你的commit
8. type并不确定，可以有自己的标准
9. 不小心用错了type,可以用git rebase -i重新编辑commit history
10. 命名规则是为了一类工具的使用？
    
#### 常用的type
1. fix: A bug fix
2. feat: A new feature
3. docs: Documentation only changes
4. build: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
5. perf: A code change that improves performance
6. refactor: A code change that neither fixes a bug nor adds a feature
7. style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
8. test: Adding missing tests or correcting existing tests

#### 支持的部分scope
(其实没太看懂)scope必须是改变的npm package的名字
1. compiler
2. http
3. router
4. **package**
特殊的 ""use package name" rule

#### examples
##### Commit message with description and breaking change footer
>feat: allow provided config object to extend other configs
>
>BREAKING CHANGE: `extends` key in config file is now used for extending other config files
##### Commit message with ! to draw attention to breaking change
>refactor!: drop support for Node 6
##### Commit message with both ! and BREAKING CHANGE footer
>refactor!: drop support for Node 6
>
>BREAKING CHANGE: refactor to use JavaScript features not available in Node 6.
##### Commit message with no body
>docs: correct spelling of CHANGELOG
##### Commit message with scope
>feat(lang): add polish language
##### Commit message with multi-paragraph body and multiple footers
>fix: correct minor typos in code
>
>see the issue for details
>
>on typos fixed.
>
>Reviewed-by: Z
>Refs #133

## Gitflow Workflow
链接：https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
- master branch 只记录稳定commit
- develop branch 则记录所有的commit
- feature branch 是最新develop branch的子brach,不直接跟master交互。
- release branch 预发布分支,develop branch积累到一定程度后fork出来，只进行一点bug的修补、文档的补充，不新加feature。准备好之后就merge进master branch并打上标签，同时也可以merge回develop branch
- hotfix branch 为master branch紧急修改bug，完成后merge到master branch和develop branch

## 本地与远程的分支同布
链接：https://www.jianshu.com/p/811b07b129e8

## tips
1. git push origin develop 把本地develop推送到所有远端branch上
2. git push origin dev:develop 把本地dev推送到远端develop上
3. git commit 自动打开文本编辑器以修改commit message
4. git remote prune origin 清除本地记录的，远端已删除的分支

## 工作流程
### 如何建立并连接repo
远端已经有repo，想要建立与本地的连接
#### 方法一
1. cd
2. git init
3. git remote add origin +repositoryLink
4. git checkout -b dev
5. git pull origin dev:develop
至此，origin源即是远端的链接，远端的develop分支已经与本地的dev分支建立关联，develop分支的内容已经保存到本地
#### 方法二
1. cd 
2. git clone repositoryLink
至此，origin源即是远端的链接，远端的master分支已经与本地的master分支建立关联，master分支的内容已经保存到本地
### 工作流程
比如想新加一个功能
1. git checkout dev  切换到dev分支
2. git checkout -b feat  创建并切换到dev分支
3. 进行修改
4. git status  常常查看仓库状态是个好习惯
5. git add \<filename\> 将修改过的file加入暂存区
6. git commit  然后会自动弹出文本框，填写了commit message后保存关闭文本框，commit完成
7. git checkout dev 回到dev分支
8. git merge feat 将feat分支合并到dev分支
9. git branch -d feat 删除feat分支
10. git push origin dev:develop 将本地dev分支，提交到远端develop分支
11. 若上一步出错，是因为远端的develop分支被别人修改过，在dev分支执行git pull，再执行上一步即可
至此，远端的develop分支已经和本地的dev分支同步在最新状态，远端的master分支则没有更新，但会自动提示是否发起PR，大多数情况下不用立即PR