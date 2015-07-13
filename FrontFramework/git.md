#git命令#
##基本操作##
###git help: ###
	git help <command>  
	git <command> -h  
	git <command> --h

###git config:###
	git config --global user.name "ljwrer"  
	git config --global user.email ljwrer@sina.com
	
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

###.gitignore###
过滤 未跟踪-->已跟踪

###git rm###
	git rm --cache//从暂存区删除
	git rm//从暂存区与工作区删除
	git rm$(git ls-files --deleted) 删除所有被跟踪，但在工作目录被删除的文件





