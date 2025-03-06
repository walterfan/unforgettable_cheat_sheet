# Git Cheat Sheet
## 1. 代码会存在于三个区域：

1) 工作区 working directory
2) 暂存区 staging area
3) 仓库区 repository

## 2. 代码从拉到推
```
git fetch origin <branch>
git merge <branch>
```
以上两条命令等于 git pull origin <branch>
```
git fetch origin <branch>
git rebase <branch>
```
以上两条命令等于 git pull origin <branch> --rebase


```
git add xxx
git commit -m "xxx"
git push origin <branch>
```

## 3. 替换本地改动
*  撤销工作区修改

```
git checkout -- <filepath>
```

*  撤销暂存区修改

```
git reset HEAD <filepath>
```

*  用远程仓库中的代码覆盖本地代码

```
git fetch remote <branchname>
git checkout remote/branch -- a/file b/file
git reset --hard remote/branch
git reset --hard HEAD
git reset <commit_hash_id> <filepath>
```


## 4. 创新切换删除分支

1) 创新分支 `git checkout -b <branchname>`

2) 切换分支 `git checkout <branchname>`

3) 删除分支 `git branch -d <branchname>`

*  删除远程分支: 

```
git push origin --delete <branchname>
   or git push origin :<branchname>
```

*  清空本地分支: `git remote prune origin`

4) 查看分支 `git branch -a`

5) 重命名分支

```
git branch -m <old_branch_name> <new_branch_name>
git push origin <new_branch_name>

#note: origin is a remote repo
git remote add <repo_name> <git_url>
```

6) 比较分支

```
git diff <branch1>..<branch2> --stat
```


## 5. 查看及修改历史

*  显示最近5次的提交的概要历史记录

```
  git log --graph --stat --oneline -5
```

*  显示最近3次的提交的详细历史记录

```
  git log -p -3
```

* 合并最近几次 commits 到一个

git rebase --interactive HEAD~2

then select the last commit and replace 'pick' with 'squash'

save this editing, then go to another editing, update the commit message and save again

* 从提交记录中删除 large file

```
git filter-branch --tree-filter 'rm -f large_file' HEAD
```

## 6. 比较区别

1) 比较工作区和暂存区 git diff
2) 比较暂存区和仓库区 git diff --cached
3) 比较工作区和仓库区 git diff HEAD
4) 比较两个分支 git diff branch_1..branch_2
5) 与 stash 的内容比较: git stash show -p stash@{1},  stash@{1} 可以省略, 默认是第一个

## 7. 管理标签

*  显示标签

```
git tag
```
*  添加标签
```
git tag -a <tagname> -m <comments>
```

*  推送标签
```
git push --tags
```
*  获取远程标签
```
git fetch origin tag <tagname>
```
*  删除标签
```
git tag -d <tagname>
git push origin :refs/tags/<tagname>
git push origin --delete tag <tagname>
```
*  检出标签对应的代码
```
git checkout tags/<tag_name>
```

## 8. 子模块和子树

*  添加子模块
```  
git submodule add https://chromium.googlesource.com/v8/v8.git third_path/v8

```
*  初始化子模块
```
git submodule init

```
*  修改子模块
```
git submodule update --init --recursive

git submodule update --init --recursive --remote --force

git submodule update --remote

git commit -a -m "update sub module" && git push origin master
```
* 更改子模块的地址

 - first, edit the .gitmodules file to update the URL and 
 - then run git submodule sync --recursive to reflect that change to the superproject and your working

```
git submodule sync --recursive
cd <submodule_dir> 

git fetch
git checkout origin/master
git branch master -f
git checkout master
```
* 删除子模块

以子模块 third_party/rapidjson 为例

```
git submodule deinit -f -- third_party/rapidjson
rm -rf .git/modules/third_party/rapidjson
git rm -f third_party/rapidjson
```

### subtree

```
git subtree add   --prefix=<prefix> <commit>
git subtree add   --prefix=<prefix> <repository> <ref>
git subtree pull  --prefix=<prefix> <repository> <ref>
git subtree push  --prefix=<prefix> <repository> <ref>
git subtree merge --prefix=<prefix> <commit>
git subtree split --prefix=<prefix> [OPTIONS] [<commit>]

```

## 9. 回退及合并

* rollback
```
git log -10
git reset xxx
git push origin <branchname> --force
```

* merge commits

```
git merge-base your_branch master
... the commit ID will be printed out as a result. Or use 'git rebase -i HEAD~N' where n is the number of commits.
git rebase -i <commit_ID_above>
... replace "pick" in the input with "squash" to squash the commit.
git push -f origin your_branch
```


## 10. 冲突解决

```
git stash
git stash pop

# resolve conflict manually, add it 
git rebase --continue

```

## FAQ

### A DETACHED HEAD ON A GIT REPOSITORY

* Create a branch called “temp” by typing: `git branch temp`
* Switch over to your new branch by checking it out: `git checkout temp`
* Point the master pointer to the temp branch pointer (the-f means force): `git branch -f master temp`
* Switch back to the master branch: `git checkout master`
* Now we delete our temp branch: `git branch -d temp`
* Push our new changes to the remote repository: `git push origin master`

### github ssh config

```
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

### How to set a release tag
* suppose branch is release/202410, tag is release/v202410
```shell
git checkout release/202410
git pull origin release/202410
git reset --hard origin/release/202410
git tag -a release/v202410 -m "release in 202410"
git push origin release/v202410

```