---
title: Git使用指定Key拉取和推送代码
date: 2023-07-07 20:00:00
tags:
---

## 在 ~/.ssh/config 中添加配置

```sh
Host github.com
    HostName github.com
    User git
    IdentityFile /Users/nbaoping/.ssh/id_rsa.github
    IdentitiesOnly yes
```


## 使用环境变量 GIT_SSH_COMMAND

```sh
GIT_SSH_COMMAND="ssh -i ~/.ssh/id_rsa_example" git clone example
```

## 在仓库clone时指定key

```sh
git clone git@github.com:yoyun/hello.git --config core.sshCommand="ssh -i ~/.ssh/you_ssh_key"
```

## 在config中指定key

```sh
git config core.sshCommand "ssh -i ~/.ssh/you_ssh_key"
```
