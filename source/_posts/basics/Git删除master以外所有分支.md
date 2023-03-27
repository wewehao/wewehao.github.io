---
title: Git删除master以外所有分支
date: 2021-02-05 19:02:27
tags:
---

## 命令

```shell
git branch | grep -v "master" | xargs git branch -D
```

## 注意

1、执行前需要切换到master分支执行
2、当前分支未做修改
