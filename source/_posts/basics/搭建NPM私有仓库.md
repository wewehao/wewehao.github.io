---
title: 搭建NPM私有仓库
date: 2021-01-07 13:29:59
tags:
---

verdaccio github: [https://github.com/verdaccio/verdaccio](https://github.com/verdaccio/verdaccio)

## 先上效果图

![image1](/images/verdaccio_1.png)

![image1](/images/verdaccio_2.png)

## 安装

1、执行命令，拉取verdaccio的docker镜像

`docker pull verdaccio/verdaccio`

2、执行命令，在用户根目录下创建config文件夹存放配置

`mkdir -p ~/config`

3、执行命令，clone项目

`git clone https://github.com/verdaccio/docker-examples`

4、执行命令，添加配置

`cd docker-examples`

`mv docker-local-storage-volume ~/config/verdaccio`

5、执行命令，添加权限

`chown -R 100:101 ~/config/verdaccio`

6、编辑文件，设置权限

> 编辑 `~/config/verdaccio/conf/config.yaml`,配置`push_user`、`admin`用户有权限访问包，`admin`用户有权限发包，其它用户无权限。

```text
packages:
  '**':
    access: push_user admin
    publish: admin
    proxy: npmjs
```

7、执行命令，启动镜像

`docker run --name verdaccio -itd -p 4873:4873 -v ~/config/verdaccio:/verdaccio ~/config/verdaccio/storage:/verdaccio/storage verdaccio/verdaccio`

## 项目安装

1、执行命令，创建`.npmrc`指定npm源

`vim .npmrc`

2、在`.npmrc`中添加以下内容

`registry=http://0.0.0.0:4873/`

3、执行命令，登陆npm私有仓库

`npm login --registry http://0.0.0.0:4873`

4、执行命令，安装项目依赖

`npm i`

## 其它

1、npm依赖安装慢可以设置淘宝源

`npm config set registry https://registry.npm.taobao.org`
