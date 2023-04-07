### 设置用户签名
```shell
git config --global user.name jieni
git config --global user.email 759835420@qq.com
```

### 初始化本地库
```shell
git init # 初始化当前目录为git仓库
```
### 查看本地库状态
```shell
git status 
```
### 添加到暂存区
```shell
git add 文件名称
git add . # 所有修改过/新添加文件加入暂存区
```
### 删除暂存区的文件
```shell
git rm --cached 文件名称
```
### 提交暂存区到本地仓库
```shell
git commit -m "备注信息"
```
### 查看提交记录
```shell
git reflog # 查看已经删除的提交记录
git log [option]
  - option
    --all 显示所有分支
    --pretty=oneline 将提交信息显示为一行
    --abbrev-commit 使得输出的commitId更简短
    --graph 以图的形式显示
```

### 版本穿梭(指向历史版本测试)
```shell
git reset --hard 版本号
```
