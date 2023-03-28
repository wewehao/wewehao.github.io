---
title: Centos服务器配置Github密钥
date: 2023-03-28 19:29:59
tags:
---

1. 打开终端并登录服务器。
  ```
  ssh root@127.0.0.1
  ```
  root为服务器账户名，127.0.0.1替换为服务器地址


2. 运行以下命令创建.ssh文件夹：
  ```
  mkdir -p ~/.ssh
  ```

3. 运行以下命令生成known_hosts文件：
  ```
  ssh-keyscan github.com >> ~/.ssh/known_hosts
  ```

4. 然后，将GitHub账户中的SSH密钥添加到服务器上，如下所示：
    - 首先，使用以下命令查看是否已经有SSH密钥：
    ```
    ls -al ~/.ssh
    ```
    - 如果没有，运行以下命令生成新的SSH密钥（按Enter键以接受默认参数）：
    ```
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```
    - 然后，将公钥添加到GitHub账户中。[https://github.com/settings/keys](https://github.com/settings/keys)

5. 最后，使用git克隆命令在服务器上拉取GitHub项目，如下所示：
  ```
  git clone git@github.com:username/repo.git
  ```
  其中，"username"是GitHub用户名，"repo"是需要克隆的项目名称。