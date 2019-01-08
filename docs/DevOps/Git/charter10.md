---
layout: default
title: 修改已提交的用户信息
nav_order: 4
parent: Git
grand_parent: DevOps
---

# 修改已提交的用户信息

## 1、克隆仓库

``` bash
git clone --bare https://gitee.com/yidane/notes.git
cd repo.git
```

## 2、命令行中执行修改命令

``` bash
#!/bin/sh

git filter-branch --env-filter '

Nxin_EMAIL="yibihao@nxin.com"
Outlook_EMAIL="yidane@outlook.com"
CORRECT_NAME="yidane"
CORRECT_EMAIL="yidane@163.com"

if [ "$GIT_COMMITTER_EMAIL" = "$Nxin_EMAIL" ]||[ "$GIT_COMMITTER_EMAIL" = "$Outlook_EMAIL" ];then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi

if [ "$GIT_AUTHOR_EMAIL" = "$Nxin_EMAIL" ]||[ "$GIT_AUTHOR_EMAIL" = "$Outlook_EMAIL" ];then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

## 3、同步到远程仓库

``` bash
git push --force --tags origin 'refs/heads/*'
```