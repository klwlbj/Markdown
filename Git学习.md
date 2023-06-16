

### 概念

工作区

暂存区

本地仓库

远程仓库

### 命令

#### 基础命令

从初始化到提交，推送的流程

```shell 
git init #初始化git仓库
git clone #clone 新项目
git fetch #下载远程变动到本地仓库
git fetch --all  #获取所有远程分支

git merge [branch] #合并分支到当前分支

git pull #拉取代码到工作区，相当于fetch+merge
    git pull origin master:bug #拉取远程master与bug分支合并
    git pull origin master 
    --rebase #不merge

git add . #添加所有文件到暂存区
git rm [file] #删除文件，井将改动放入暂存区
	git rm -cached [file] #包留文件于工作区，但以后停止追踪该文件
git mv [file1] [file2] #改名文件，加入暂存区

git checkout [branch] #切换分支
	git checkout .   #取消所有修改
	git checkout -- [file]  #取消暂存区某个文件的修改
	#1.文件自修改后还没有add到暂存区，现在，撤销修改就回到和版本库一模一样的状态；2.文件已经add到暂存区后，又作了修改，现在，撤销修改就回到add后的状态。
	git checkout -b 分支名 #用当前分支创建新分支,并切换分支
	
git reset HEAD [file]  #回退提交到上一个版本，可指定文件

git branch #显示本地分支
	git branch -a #显示本地及远程分支
	git branch [branch] #创建新分支
	git branch -v #显示分支列表及对应的最近提交记录
	git branch -vv #显示分支列表及对应的最近提交记录，以及对应远程分支名
	git branch -d [branch]  #删除本地分支
	git branch -m [branch] #重命名当前分支
	
git diff # 显示差异

git push origin [branch] #推送本地分支到远端仓库
	git push origin --delete [branch] #删除远程分支

git status #查看工作区文件状态

git log #查看版本库修改记录,按键盘q 退出
	git log --graph #图形展示树分支日志

git rebase #变基
	git rebase -i [loghash]   #从某提交起修改提交记录

git commit -m [message] #提交
	git commit -a #提交工作区与暂存区的变化直接到仓库
	git commit --amend #提交代码，附加到上一次提交内，不创建新提交


git cherry-pick
	git cherry-pick [commit hash] #挑选任意分支上的某个提交来合并
	git cherry-pick [branch] #挑选指定分支上的最新提交至本分支
git update-index --skip-worktree /path/to/file  # 忽略本地修改
```

全局配置

```shell
git config --global pull.rebase true

git config --global rebase.autoStash true

```



### 查看和配置远端

```shell
git remote -v #查看远端
git remote set-url origin git@github.com:username/repo.git  #设置为ssh方式

```

stash

rebase
