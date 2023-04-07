### 分支版本
```shell
git branch -v
```

### 查看所有分支
```shell
git branch -a
```

### 创建分支
```shell
git branch 分支名 # 创建本地分支
git branch -b 分支名 # 直接切换到一个不存在的分支(创建并切换)
```

### 切换分支
```shell
git checkout 分支名
```

### 合并分支（正常合并）
```shell
git merge 分支名
```
### 删除分支
```shell
# 不能删除当前分支,只能删除其他分支
git branch -d b1 # 删除分支时，需要做各种检查
git branch -D b1 # 不做任何检查，强制删除
```
