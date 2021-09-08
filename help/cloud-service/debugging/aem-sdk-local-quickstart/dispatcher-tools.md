---
title: 调试Dispatcher工具
description: “调度程序工具”提供了一个容器化的Apache Web Server环境，该环境可用于在本地模拟AEM作为Cloud Services的AEM发布服务的调度程序。 调试AEM工具的日志和缓存内容对于确保端到端Dispatcher应用程序和支持缓存和安全配置正确至关重要。
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---


# 调试Dispatcher工具

“调度程序工具”提供了一个容器化的Apache Web Server环境，该环境可用于在本地模拟AEM作为Cloud Services的AEM发布服务的调度程序。

调试AEM工具的日志和缓存内容对于确保端到端Dispatcher应用程序和支持缓存和安全配置正确至关重要。

>[!NOTE]
>
>由于调度程序工具基于容器，因此每次重新启动它时，以前的日志和缓存内容都会被销毁。

## Dispatcher工具日志

可通过`stdout`或`bin/docker_run`命令获取Dispatcher工具日志，或者通过`/etc/https/logs`的Docker容器获取更多详细信息。

有关如何直接访问Dispatcher工具Docker容器日志的说明，请参阅[Dispatcher日志](./logs.md#dispatcher-logs)。

## Dispatcher工具缓存

### 访问Docker容器中的日志

Dispatcher缓存可以直接在位于` /mnt/var/www/html`的Docker容器中访问。

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

调度程序日志可以从位于`/mnt/var/www/html`的Docker容器复制到本地文件系统，以便使用您喜爱的工具进行检查。 请注意，这是一个时间点副本，不提供对缓存的实时更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

