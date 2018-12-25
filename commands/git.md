##git常用命令

### 分支操作

0. 把本地已存在的仓库和远程仓库关联

	> git remote add origin git@github.com:michaelliao/learngit.git

1. 远程拉取本地不存在的分支

	> git checkout -b [分支名] [远程名]/[分支名]
	
	> 这个将会自动创建一个新的本地分支，并与指定的远程分支关联起来。

2. 修改提交历史

	> git rebase -i HEAD~3
	
	> 表示要修改当前版本的倒数第三次状态。
	
3. 拉取远程代码

	- //从远程的origin的master主分支上获取最新版本到origin/master分支上
	
	> git fetch origin master
	
	- //比较本地的master分支和origin/master分支的区别
	
	> $:git log -p master..origin/master 

	- //合并
	
	> $:git merge origin/master
	
4. 查看本地和远程的关联关系

	> git branch -vv
	
### 问题汇总

1. You have not concluded your merge (MERGE_HEAD exists)

	- 解决办法一:保留本地的更改,中止合并->重新合并->重新拉取
	
	> $:git merge --abort
	
	> $:git reset --merge
	
	> $:git pull
	
	- 解决办法二:舍弃本地代码,远端版本覆盖本地版本(慎重)
	
	> $:git fetch --all
	
	> $:git reset --hard origin/master

	> $:git fetch

