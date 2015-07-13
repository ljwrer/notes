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
	//从暂存区删除
	git rm
	//从暂存区与工作区删除
	git rm$(git ls-files --deleted) 
	//删除所有被跟踪，但在工作目录被删除的文件

###git commit###
	git commint -m "message"
	//暂存区-->提交区
	git commit -a -m "message"
	//工作区-->提交区
    
###git log###
	git log
	git log -- oneline
	git config --global alias.lg "log --graph --pretty=format:'%Cred%H%Creset @%C(yellow)%d%Creset %n Author: %cn <%_ce> %n Date: %_cd %_Cblue(%cr)%Creset %n %n Commit subject: %Cgreen%s%Creset %n'"

###git alias###
	//设置别名
	git config alias.shortname <fullcommand>

###git diff###
	git diff
	//工作目录与暂存区区别
	git diff-cached[<reference>]
	//暂存区与某次提交差异，默认为HEAD,即当前提交，可以不填
	git diff <reference>
	//工作目录与某次提交的差异
	git diff <commitID1> <commitID2>
	//两次提交的区别

###git checkout###
	git checkout --<file>
	暂存区-->工作目录
	将会丢弃工作目录的内容