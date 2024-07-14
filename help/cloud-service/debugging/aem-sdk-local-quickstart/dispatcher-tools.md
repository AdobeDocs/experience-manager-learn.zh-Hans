---
title: 调试Dispatcher Tools
description: Dispatcher Tools提供了一个容器化的Apache Web Server环境，可用于在本地模拟AEM as a Cloud Service的AEM Publish服务的Dispatcher。 调试Dispatcher工具的日志和缓存内容对于确保端到端AEM应用程序以及支持缓存和安全配置的正确性可能至关重要。
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 调试Dispatcher Tools

Dispatcher Tools提供了一个容器化的Apache Web Server环境，可用于在本地模拟AEM as a Cloud Service的AEM Publish服务的Dispatcher。

调试Dispatcher工具的日志和缓存内容对于确保端到端AEM应用程序以及支持缓存和安全配置的正确性可能至关重要。

>[!NOTE]
>
>由于Dispatcher Tools是基于容器的，因此每次重新启动它时，都会销毁以前的日志和缓存内容。

## Dispatcher Tools日志

Dispatcher工具日志可通过`stdout`或`bin/docker_run`命令获得，或者在`/etc/https/logs`的Docker容器中提供更详细的信息。

有关如何直接访问Dispatcher工具的Docker容器日志的说明，请参阅[Dispatcher日志](./logs.md#dispatcher-logs)。

## Dispatcher工具缓存

### 访问Docker容器中的日志

可直接在` /mnt/var/www/html`处的Docker容器中访问Dispatcher缓存。

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

可以使用您喜爱的工具将Dispatcher日志从`/mnt/var/www/html`处的Docker容器复制到本地文件系统以进行检查。 请注意，这是一个时间点副本，不提供对缓存的实时更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
