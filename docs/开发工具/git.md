## git

下载地址:[https://git-scm.com/download](https://git-scm.com/download)

TortoiseGit:[https://download.tortoisegit.org/tgit/](https://download.tortoisegit.org/tgit/)

[小乌龟百度网盘]( https://blog.csdn.net/x1562092008/article/details/102842286 )

[登录]([https://blog.csdn.net/qq_28584685/article/details/53781473](https://blog.csdn.net/qq_28584685/article/details/53781473))

```
1. 设置用户名
git config --global user.name 'xinyesunsf' 
2. 设置用户名邮箱
git config --global user.email 'fandexilu@aliyun.com'

//设置不用每次登录都验证
```

## github

[入门傻瓜式操作](http://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247487711&idx=1&sn=47b5354bd0f7a4744480ea421aeaa8fd&chksm=97ea3abaa09db3ac8992fa82b0ea048b03662ea2094440bc13d4aa7c29576563de9303cab22d&mpshare=1&scene=1&srcid=1116Q2EFl3YctFwl3n4ER2ij#rd)

```
//本地远程生成一个项目
git clone https://github.com/sanqianhuanyu/JavaStudy.git
git remote add origin https://github.com/sanqianhuanyu/JavaStudy.git
//初始化
git init
//新增文件
git add README.md
git commit -m "first commit" 
//推送到远程分支
git push -u origin master
```

### 解决每次登陆

[github 登陆](https://zhuanlan.zhihu.com/p/81334170)

[配置公钥](https://www.cnblogs.com/qcwblog/p/5709720.html)

```shell
## 移除原有地址
git remote rm origin

## 添加ssh 地址
git remote add origin git@github.com:xinyessf/haiNanDoc.git

## 设置推送方式是 ssh 的方式
git push --set-upstream origin master

## 推送至远程仓库
git push origin master


## 重新设置ssh 登录的方式
ssh-keygen -t rsa -C "fandexilu@aliyun.com"


##
git@github.com:xinyessf/big-data-soa.git
git@github.com:xinyessf/JavaStudy.git
git@github.com:xinyessf/haiNanDoc.git
```



```

```



## git开发用到命令

### 解决下载慢的问题

```
https://zhuanlan.zhihu.com/p/102409790
```



### git分支文件

```
//到对应目录下查看分支
git checkout -b slave1  //创建并切换分支  -b表示切换
git checkout -b 本地分支名x origin/远程分支名x
git checkout -b dev origin/dev，作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
git branch dev  //创建分支
git branch      //列出所有分支
git branch -d name //删除分支
git checkout name   //切换分支
git branch -a  //查看所有分支,包括远程分支

git checkout dev_20210819_constant
```

### git文件增删

```
git status       //查看当前文件的状态
git add xx       //添加需要提交的文件
git add .  添加所有修改的文件
git commit -m '第一次提交'  //提交
git diff 文件   //查看不同
-- 添加后如何移除呢<br>git rm –cached 文件名  //移除

```

### 查看日志

```
git log
git log --oneline //单行查看日志
git log –pretty=oneline  -n查看最近几次的commit-ID：


```

### 版本

```
git reset --hard HEAD^   //回退到上一版本
---合并分支到 master
git merge master  //合并分支到master
```

### 分支

```
git stash //将当前的工作现场隐藏起来
git status  //查看状态
git checkout -b issue-404  //创建404分支
//修改提交
//切换到master分支
//修复完成后合并分支到master
git merge --no-ff -m "merge bug issue-404"
//然后在master中删除临时分支
git branch -d issue-404
//回到自己的分支干活
git checkout dev_20210819_constant
git stash list   //查看临时分支
//删除
git stash  pop  //删除的同时,恢复stash
```

### 推送

```
git pull //pull成功了,但是要解决冲突
git pull --rebase    -- rebase出现问题了 执行 git rebase --abort
git push origin dev_20210819_constant

git pull origin dev_20210819_constant
git branch --set-upstream-to=origin/dev_20210819_constant
```

### bug分支

```
git stash //将当前的工作现场隐藏起来
git status  //查看状态
git checkout -b issue-404  //创建404分支
//修改提交
//切换到master分支
//修复完成后合并分支到master
git merge --no-ff    master
//然后在master中删除临时分支
git branch -d issue-404
//回到自己的分支干活
git checkout dev
git stash list   //查看临时分支
//删除
git stash  pop  //删除的同时,恢复stash
git stash list log //查看
git stash apply
```