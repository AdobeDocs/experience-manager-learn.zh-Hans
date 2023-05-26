---
title: 调试Dispatcher工具
description: Dispatcher工具提供了一个容器化的Apache Web Server环境，可用于在本地模拟AEM作为Cloud Services的AEM Publish服务的Dispatcher。 调试Dispatcher工具的日志和缓存内容对于确保端到端的AEM应用程序以及支持缓存和安全配置的正确性可能至关重要。
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 调试Dispatcher工具

Dispatcher工具提供了一个容器化的Apache Web Server环境，可用于在本地模拟AEM作为Cloud Services的AEM Publish服务的Dispatcher。

调试Dispatcher工具的日志和缓存内容对于确保端到端的AEM应用程序以及支持缓存和安全配置的正确性可能至关重要。

>[!NOTE]
>
>由于Dispatcher工具是基于容器的，因此每次重新启动它时，先前的日志和缓存内容都会被销毁。

## Dispatcher工具日志

Dispatcher工具日志可通过 `stdout` 或 `bin/docker_run` 命令或在Docker容器中提供的更多详细信息，位于 `/etc/https/logs`.

参见 [Dispatcher日志](./logs.md#dispatcher-logs) 有关如何直接访问Dispatcher工具的Docker容器日志的说明。

## Dispatcher工具缓存

### 访问Docker容器中的日志

Dispatcher缓存可以直接在Docker容器中访问，网址为 ` /mnt/var/www/html`.

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

可以从Docker容器中复制调度程序日志，位置为 `/mnt/var/www/html` 到本地文件系统以使用您喜爱的工具进行检查。 请注意，这是一个时间点副本，不会提供对缓存的实时更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
