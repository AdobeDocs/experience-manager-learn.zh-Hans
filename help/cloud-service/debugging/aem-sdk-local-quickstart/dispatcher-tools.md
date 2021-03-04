---
title: 调试调度程序工具
description: 调度程序工具提供了一个容器化的Apache Web Server环境，可用于将AEM模拟为Cloud Services的AEM Publish服务的本地调度程序。 调试调度程序工具的日志和缓存内容对于确保端到端AEM应用程序以及支持缓存和安全配置正确至关重要。
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: 开发
role: 开发人员
level: 初学者，中级
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---


# 调试调度程序工具

调度程序工具提供了一个容器化的Apache Web Server环境，可用于将AEM模拟为Cloud Services的AEM Publish服务的本地调度程序。
调试调度程序工具的日志和缓存内容对于确保端到端AEM应用程序以及支持缓存和安全配置正确至关重要。

>[!NOTE]
>
>由于调度程序工具基于容器，因此每次重新启动它时，以前的日志和缓存内容都会被销毁。

## 调度程序工具日志

调度程序工具日志可通过`stdout`或`bin/docker_run`命令访问，或在位于`/etc/https/logs`的Docker容器中提供更多详细信息。

有关如何直接访问Dispatcher Tools的Docker容器日志的说明，请参阅[ Dispatcher logs](./logs.md#dispatcher-logs)。

## 调度程序工具缓存

### 访问Docker容器中的日志

调度程序缓存可以直接访问位于` /mnt/var/www/html`的Docker容器。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### 将Docker日志复制到本地文件系统

调度程序日志可以从位于`/mnt/var/www/html`的Docker容器中复制到本地文件系统，以便使用您喜爱的工具进行检查。 请注意，这是一个时间点拷贝，不提供缓存的实时更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

