---
title: 使用日志调试AEM SDK
description: 日志是调试AEM应用程序的前线，但取决于部署的AEM应用程序中是否有足够的日志记录。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# 使用日志调试AEM SDK

访问AEM SDK的日志(AEM SDK本地快速入门Jar或Dispatcher工具)可以提供有关调试AEM应用程序的重要见解。

## AEM日志

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

日志是调试AEM应用程序的前线，但取决于部署的AEM应用程序中是否有足够的日志记录。 Adobe建议尽可能保持本地开发和AEMas a Cloud Service开发日志记录配置相似，因为它在AEM SDK的本地快速启动和AEMas a Cloud Service的开发环境中规范了日志可见性，从而减少了配置扭曲和重新部署。

此 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 通过Sling Logger OSGi配置在AEM应用程序的Java包的DEBUG级别配置日志记录，以供本地开发

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

哪些记录到 `error.log`.

如果默认日志记录不足以用于本地开发，则可以通过AEM SDK的本地快速启动日志支持Web控制台( )配置临时日志记录。[/system/console/slinglog](http://localhost:4502/system/console/slinglog))，但是不建议将临时更改保留到Git，除非在AEMas a Cloud Service的开发环境中也需要这些相同的日志配置。 请记住，通过日志支持控制台所做的更改将直接保留到AEM SDK的本地快速入门存储库。

Java log语句可在 `error.log` 文件：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，“跟踪”以下对象会很有用 `error.log` 将输出流传输到终端。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要 [第三方尾部应用程序](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 或使用 [Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936).

## Dispatcher日志

在以下情况下，Dispatcher日志将输出到stdout `bin/docker_run` 将会调用，但可以通过在Docker容器中直接访问日志。

### 访问Docker容器中的日志{#dispatcher-tools-access-logs}

Dispatcher日志可以直接在Docker容器中访问，网址为 `/etc/httpd/logs`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_此 `<CONTAINER ID>` 在 `docker exec -it <CONTAINER ID> /bin/sh` 必须替换为中列出的目标Docker容器ID `docker ps` 命令。_


### 将Docker日志复制到本地文件系统{#dispatcher-tools-copy-logs}

可以从Docker容器中复制调度程序日志，位置为 `/etc/httpd/logs` 使用您喜爱的日志分析工具检查本地文件系统。 请注意，这是一个时间点副本，不提供日志的实时更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_此 `<CONTAINER_ID>` 在 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 必须替换为中列出的目标Docker容器ID `docker ps` 命令。_
