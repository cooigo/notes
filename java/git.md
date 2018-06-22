# git 使用

```
初始化
git init

添加暂存区
git add \*

提交到分支
git commit -m "xxx"

查看分支
git branchbranch

设置分支于远程分支关联
git branch --set-upstream-to=origin/dev dev

查看远程库信息
git remote -v

从本地推送分支
git push origin branch-name

从远程抓取分支
git pull

在本地创建和远程分支对应的分支
git checkout -b branch-name origin/branch-name

创建+切换分支
git checkout -b dev

切换分支
git checkout master

合并分支[fast forward]
git merge branch-name

合并有历史分支[--no-ff]
git merge --no-ff -m "merge with no-ff" dev

删除分支
git branch -d branch-name

创建标签
git tag [name]

查看标签
git tag

日志
git log --graph --pretty=oneline --abbrev-commit

执行命令日志
git reflog

当前状态
git status

临时存储
git stash

临时存储（调出、列表、删除）
git stash [pop/list/drop]

回退主版本
git reset --hard HEAD^

回退具体的版本
git reset --hard 102af

撤销暂存区修改
git reset HEAD readme.txt

撤销工作区修改
git checkout -- readme.txt
```
