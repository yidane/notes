---
layout: default
title: 同时提交代码到多个远程仓库
nav_order: 4
parent: Git
grand_parent: DevOps
---

# **同时提交代码到多个远程仓库**

## 1 场景描述

计划将本笔记开源，同时提交到github和gitee上

## 2 实现步骤

### 2.1 配置秘钥和公钥

#### 2.1.1 查看秘钥

``` bash
ls ~/.ssh
```

如果存在github,github.pub,gitee,gitee.pub，说明已经存在相应秘钥和公钥

#### 2.1.2 创建秘钥和公钥

``` bash
ssh-kengen -t rsa -f ~/.ssh/gitee -C "yidane@163.com"
ssh-kengen -t ras -f ~/.ssh/github -C "yidane@163.com"
```

其中，gitee和github为相对应的秘钥名称。由于本人github和gitee邮箱相同，故此处邮箱都为 yidane@163.com。操作完毕之后，~/.ssh目录下新增gitee gitee.pub和github github.pub两组文件。

#### 2.1.3 配置秘钥

* 在～/.ssh文件夹下创建config文件

``` bash
touch config
vim config
```

* 编辑config文件，配置不同的仓库指向不同的秘钥文件

``` bash
# github config
Host github.com
HostName github.com
User git
IdentityFIle ~/.ssh/github

# gitee config
Host gitee.com
HostName gitee.com
User git
IdentityFile ~/.ssh/gitee
```

> * 原理分析
>
> * ssh客户端是通过类似git@github.com:githubUserName/repName.git的地址来识别使用本地哪个私钥的，地址中User是@前面的git，Host是@后面的github.com。
>
> * 如果所有账户的User和Host都为git和github.com，那么就只能使用一个私钥。所以要对User和Host进行配置，让每个账号使用自己的Host，每个Host的域名做CNAME解析到github.com。

#### 2.1.4 导入秘钥

``` bash
cat github.pub
cat gitee
```

复制输出的值，分别导入到github和gitee账号中的SSH keys中保存

#### 2.1.5 设置SSH agent

清空本地的SSH缓存，添加新的SSH秘钥到SSH agent中

``` bash
ssh-add -D
ssh-add github
ssh-add gitee
```

确认新秘钥添加成功

``` bash
ssh-add -l
```

#### 2.1.6 测试ssh链接

``` bash
$ ssh -T git@github.com
Hi yidane/notes! You've successfully authenticated, but GitHub does not provide shell access.
$ ssh -T git@gitee.com
Hi yidane/notes! You've successfully authenticated, but GitHub does not provide shell access.
```

如果出现上述信息，则表示连接成功

### 2.2 配置远程仓库

#### 2.2.1 全局用户名邮箱配置

``` bash
# 取消全局 用户名/邮箱 配置
$ git config --global --unset user.name
$ git config --global --unset user.email

# 进入项目文件夹，单独设置每个repo 用户名/邮箱
$ git config user.email "yidane@163.com"
$ git config user.name "yidane"

# 查看git项目的配置
$ git config --list
```

#### 2.2.2 重建远程仓库地址

``` bash
# 进入项目目录后
$ git remote rm origin

# 配置github远程仓库地址
$ git remote add origin git@github.com/yidane/notes.git

# 查看配置github远程仓库后地址效果
$ git remote -v
origin    https://github.com/yidane/notes.git (fetch)
origin    https://github.com/yidane/notes.git (push)

# 配置gitee远程仓库地址
$ git remote set-url --add origin https://gitee.com/yidane/notes.git

# 查看配置github和gitee远程仓库地址后效果
$ git remote -v
origin    https://github.com/yidane/notes.git (fetch)
origin    https://github.com/yidane/notes.git (push)
origin    https://gitee.com/yidane/notes.git (push)
```

> 此处，origin的fetch地址为github的地址，即为先添加的地址；后续通过set-url --add添加的地址，都只有push功能。因此，若要调整从gitee上fetch，则应该先添加gitee的仓库地址，然后配置其他仓库地址。

#### 2.2.3 pull数据

``` bash
# 从远程仓库pull数据
$ git pull origin master
# 若出现unrelated histories错误，则使用下面命令pull数据
$ git pull origin master --allow-unrelated-histories
```

#### 2.2.4 push数据

``` bash
# 添加README.md文件
$ git add README.md
$ git commit -m "first commit"
$ git push -u origin master

# 若出现如下错误，则使用强制提交命令。注意：强制提交命令会覆盖gitee仓库数据
# hint: Updates were rejected because the remote contains work that you do
# hint: not have locally. This is usually caused by another repository pushing
# hint: to the same ref. You may want to first integrate the remote changes
$ git push -f
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 663 bytes | 663.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To https://github.com/yidane/notes.git
   afeda66..f5a3036  master -> master
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 663 bytes | 663.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Powered by Gitee.com
To https://gitee.com/yidane/notes.git
   afeda66..f5a3036  master -> master
# 从输出信息可以看出，代码向两个远程仓库提交数据，且都提交成功
```