---
title: Docker运行Redis
date: 2022-05-12 21:17:09
tags:
---

Docker是一种容器化平台，可用于在不同环境中运行应用程序。Redis是一种流行的开源内存数据库，它被广泛用于缓存和消息传递等场景。本文将介绍如何使用Docker启动Redis容器。

## 步骤

1. 拉取Redis镜像：打开终端，并输入以下命令来拉取Redis镜像。 ``` docker pull redis ```

2. 启动Redis容器：使用以下命令来启动Redis容器。

```
docker run --name my-redis -p 6379:6379 -d redis redis-server --appendonly yes
```

解析：

* `--name my-redis`：为容器指定一个名称。
* `-p 6379:6379`：将主机的6379端口映射到容器的6379端口。
* `-d redis`：在后台运行Redis容器。
* `redis-server --appendonly yes`：运行Redis服务器，并开启AOF持久化模式。

3. 测试Redis容器：

通过以下命令连接Redis容器并检查是否正常工作。

```
docker exec -it my-redis redis-cli
```

如果成功连接到Redis容器，则会看到以下提示：

```
127.0.0.1:6379>
```

现在可以尝试运行一些Redis命令来确保它正在工作。

4. 停止Redis容器

```
docker stop my-redis
```
