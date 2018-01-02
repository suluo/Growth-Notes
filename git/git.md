#### 

git branch

```
$ git branch # 列出本地所有分支
$ git branch -a # 列出所有分支
$ git branch -r # 列出远程分支
$ git branch -vv # 查看分支映射关系

$ git branch mybranch # 创建分支
$ git branch -d mybranch # 删除分支
$ git branch -D mybranch # 强制删除分支

# 建立当前分支与远程分支的映射关系
$ git branch -u origin/addFile  
$ git branch --set-upstream-to origin/addFile
$ git branch --unset-upstream #撤销本地分支与远程分支的映射关系
```

git checkout

```
$ git checkout mybranch # 切换分支
$ git checkout -b branch_name # 创建并切换
$ git checkout -b branch_name 139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96   # 以id创建并切换
$ git checkout -b 本地分支名x origin/远程分支名x # 拉取远程分支并创建本地分支
```

git fetch

```
$ git fetch origin 远程分支名x:本地分支名x   # 拉取远程分支并创建本地分支
```

git merge

```
$ git merge mybranch
# 如果冲突====上半部分是head， 下半部分是mybranch内容
```

git status

```
$ git status
$ git status -uno
```

git  add

git commit

```
$ git commit -m "message"
$ git commit -a
$ git commit -a -amend  # 对最近一次commit进行修改
$
```

git tag

```
$ git tag    # 在控制台打印出当前仓库的所有标签
$ git tag -l "v1.1.*"  # 搜索符合模式的标签
## git标签分为两种类型：轻量标签和附注标签。轻量标签是指向提交对象的引用，附注标签则是仓库中的一个独立对象。建议使用附注标签。
$ git tag v0.1.2-light  # 创建轻量标签
$ git tag -a v0.1.2 -m “0.1.2版本”   # 创建附注标签
$ git tag -a v0.1.1 9fbc3d0   # 给指定的commit打标签

$ git checkout [tagname] # 切换到标签
$ git show v0.1.2 # 查看标签版本信息
$ git tag -d v0.1.2 # 删除标签

$ git push origin v0.1.2 # 将v0.1.2标签提交到git服务器
$ git push origin –tags # 将本地所有标签一次性提交到git服务器
```

#### 回退

```
$ git reset HEAD^  # 撤销最近一次提交(即退回到上一次版本)并本地保留代码

$ git log     # 查看历史版本 假设id=139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96
# 回退
$ git reset --hard 139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96
# 修改推到远端服务器
$ git push -f -u origin master
```



