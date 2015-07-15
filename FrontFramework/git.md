#git命令#
##基本操作##
###git help: ###
	git help <command>  
	git <command> -h  
	git <command> --h

###git config:###
	git config --global user.name "ljwrer"  
	git config --global user.email ljwrer@sina.com
	//位于.gitconfig文件内
	
###git init:###
	git init

###git status:###
	git status

####内容状态####
- 工作目录
- 暂存区
- 提交区

####文件状态####
- 未跟踪
- 已跟踪

###git add###
未跟踪-->已跟踪
工作目录-->暂存区

	git add
###.gitignore###
过滤 未跟踪-->已跟踪

###git rm###
	git rm --cache
	#从暂存区删除
	git rm
	#从暂存区与工作区删除
	git rm$(git ls-files --deleted) 
	#删除所有被跟踪，但在工作目录被删除的文件

###git commit###
	git commint -m "message"
	#暂存区-->提交区
	git commit -a -m "message"
	#工作区-->提交区
    
###git log###
	git log
	git log -- oneline
	git config --global alias.lg "log --graph --pretty=format:'%Cred%H%Creset @%C(yellow)%d%Creset %n Author: %cn <%_ce> %n Date: %_cd %_Cblue(%cr)%Creset %n %n Commit subject: %Cgreen%s%Creset %n'"

###git alias###
	#设置别名
	git config alias.shortname <fullcommand>

###git diff###
	git diff
	#工作目录与暂存区区别
	git diff --cached[<reference>]
	#暂存区与某次提交差异，默认为HEAD,即当前提交，可以不填
	git diff <reference>
	#工作目录与某次提交的差异
	git diff <commitID1> <commitID2>
	#两次提交的区别
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitDiff.png)

###git checkout###
	git checkout --<file>
	#暂存区-->工作目录
	#将会丢弃工作目录的内容

###git reset###
	git reset HEAD <file>
	#将文件内容从上次提交复制到暂存区，撤销暂存区内容
	
###git checkout HEAD --<file>###
	git checkout HEAD --<file>
	#将文件内容从上次提交复制到工作目录，撤销全部改动
	！暂存区内容也会被覆盖，慎用！

##小结##
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitStatus.png)

一个Git项目中文件的状态大概分成下面的两大类，而第二大类又分为三小类：
- 未被跟踪的文件（untracked file）
- 已被跟踪的文件（tracked file）


- 被修改但未被暂存的文件（changed but not updated或modified）
- 已暂存可以被提交的文件（changes to be committed 或staged）
- 自上次提交以来，未修改的文件(clean 或 unmodified)

大家可以看到“nothing to commit (working directory clean)”；如果一个工作树（working tree）中所有的修改都已提交到了当前分支里（current head），那么就说它是干净的（clean），反之它就是脏的(dirty)。

##分支操作##
###git branch###
	#创建分支
	git branch <branchName>
	#删除分支
	git branch -d <branchName>
	#查看所有分支,*当前分支，即HEAD指向的分支
	git branch -v
git commit以后，master/branchName 指向最新的提交，HEAD也指向最新的提交
git branch next以后，next指向HEAD指向的引用
再次commit以后master和HEAD随commit移动，但next不会移动，除非HEAD切换到next
###git checkout###
	#HEAD直接指向分支 
	git checkout <barnchName>
	#创建分支，HEAD指向分支
	git checkout -b <branchName>
	#HEAD指向引用对象，如commitID
	git checkout <reference>
	！master/branchName不会跟随
	！指向没有分支所在的commitID将导致detached HEAD，此时应尽量避免再commit操作，以免当HEAD再次移动时，失去引用，被垃圾回收
	！detached HEAD只建议读取内容，此时，可以观察到提交时工作目录和暂存区的内容
	#恢复到上一个分支
	git checkout -
###git reset###
	#默认 将HEAD/master或HEAD/branchName移动到该提交区，同时将内容复制到暂存区
	git reset --mixed commitID
	#将HEAD/master或HEAD/branchName移动到该提交区，同时将内容复制到暂存区以及工作目录
	git reset --hard commitID
    #将HEAD/master或HEAD/branchName移动到该提交区，保持暂存区以及工作目录
	git reset --soft commitID
	！reset以后，由于后面的提交丢失引用，可能会被垃圾回收
	！git log也不再显式后面的提交信息
###git reflog###
	#列出所有经过的commit路径
	git reflog
	！该log信息为动态信息，如需回退，应尽快查看
###捷径###
	A^
	#A上的父提交
	A~n
	#在A之前的第n次提交
	#A可以为commitID，HEAD，master,branchName，^ ~n可叠加使用
###checkout vs reset
![](http://7xkcnd.com1.z0.glb.clouddn.com/resetVScheckout.png)

###git stash###
	#当工作内容没有提交时切换分支，checkout分支会被阻止
	#stash拥有独立的栈，即stash区
	
	#保存当前的工作目录和暂存区目录到stash区，并返回干净的工作空间
	git stash save "message"
	#查看stash收藏记录
	git stash list
	#恢复指定stash记录到工作空间
	git stash apply stash@{0}

	!stash@{n}因为shell原因可能报错，此时改为'stash@{n}'即可
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitStash.png)

	#丢弃stash指定记录
	git stash drop stash@{0}
	#恢复指定stash记录到工作空间,并丢弃改指定记录
	git stash pop stash@{0}
	#stash pop=stash apply+stash drop
###git cat-file###
	#显示git内部对象具体信息
	git cat-file -p HEAD
	#tree 当前提交信息
	#parent 上一次提交信息
###git merge###
####合并分支####
	#将当前分支与指定分支，以及两个分支的共同祖先分支合并
	#保存到工作目录和暂存区
	#创建新的提交，新的提交父指向为两个合并的分支
	#当前分支指向新的提交
	git merge branchName
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitMerge.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitMerge2.png)

####merge conflicts####
	#合并冲突
	git status
	#找到冲突文件
	#解决冲突并提交
	git add .
	git commit -m "resolved"
####merge fast-forward####
	#创建新的分支以后，msater分支未移动，转到next分支进行开发并提交
	#合并将不会有冲突发生，只是将HEAD/master移到next所在的提交，提交历史变为线性
	！不会记录合并
![](http://7xkcnd.com1.z0.glb.clouddn.com/mergeFast-forward.png)	

	避免触发fast-forward 模式
	git merge next --no-ff
![](http://7xkcnd.com1.z0.glb.clouddn.com/no-ff.png)

###git rebase###
	#HEAD位于feature分支
	git rebase master 
	#找到共同节点与分支节点之间的所有提交，并重演到master分支后
	！重演不是复制，commitID不同
	#将HEAD/feature移动到最新的提交，提交变为线性
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitRebase2.png)	
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitRebase2.png)

	#选择重演对象
	git rebase onto master --commitID
	#找到指定节点与分支节点之间的所有提交，并重演到master分支后
	#将HEAD/feature移动到最新的提交，提交变为线性
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitRebaseOnto.png)

###rebase vs merge###
- rebase不会记录合并记录
- 不要在共有分支上使用rebase,如在master上rebase将导致产生两个master,合并时也会存在相同的内容  
![](http://7xkcnd.com1.z0.glb.clouddn.com/rebaseVSmerge.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/rebaseMaster.png)

###git tag###
	#设置标签
	git tag tagName commitID

##远程操作##
以本地HEAD/master为例
###local remote server###
	#初始化一个裸仓库
	git init ~/git-server --base
	！没有工作目录

###git remote###
	#添加远程仓库
	git remote add [shortname] [url]	
	#查看远程仓库
	git remote -v
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitRemote.png)

###git push###
	#将提交历史复制到中央服务器
	#中央服务器HEAD/master向前移动
	git push serverUrl branchName
	!branch为中央服务器分支
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitPush.png)

- 本地的提交有一个隐藏的origin/master	
- origin/master不会跟随commit移动
- origin/master与中央服务器不一致时，push之前需要先同步代码

###git fetch+git merge###
当远程库的提交先于本地库时,需要fetch+merge
![](http://7xkcnd.com1.z0.glb.clouddn.com/BeforeFetch.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/BeforeFetch2.png)

	#同步远程origin/master 到本地 origin/master
	git fetch origin master
	#即复制远程origin/master到本地，且本地origin/master指向该复制
![](http://7xkcnd.com1.z0.glb.clouddn.com/fetch.png)

	#将本地HEAD/master与本地origin/master合并	
	git merge orign master
![](http://7xkcnd.com1.z0.glb.clouddn.com/mergeOrigin.png)

	#将本地历史复制到远程，远程将保留多个作者的历史
	git push origin master
###git pull###
	#git fetch+git merge
	git pull origin master
	#相当于 git fetch remote+git merge orgin master
![](http://7xkcnd.com1.z0.glb.clouddn.com/gitPull.png)

###git clone###
	#复制远程仓库到本地指定目录
	git clone remoteUrl localDirectory 
	#相当于git init+git remote+git pull
	#自动配置信息
	