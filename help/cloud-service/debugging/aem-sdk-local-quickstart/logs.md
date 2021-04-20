---
title: 使用日志调试AEM SDK
description: 日志是调试AEM应用程序的前线，但取决于部署的AEM应用程序中是否有足够的日志记录。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# 使用日志调试AEM SDK

访问AEM SDK的日志(AEM SDK本地快速启动Jar或调度程序工具)可以提供调试AEM应用程序的关键洞察。

## AEM日志

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

日志是调试AEM应用程序的前线，但取决于部署的AEM应用程序中是否有足够的日志记录。 Adobe建议将本地开发和AEM作为Cloud Service开发人员日志配置保持尽可能的相似，因为它将AEM SDK的本地快速启动和AEM上的日志可见性规范为Cloud Service开发人员环境，从而减少配置扭曲和重新部署。

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)将AEM应用程序的Java包的日志记录配置为在DEBUG级别，以便通过位于

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

它登录到`error.log`。

如果默认日志记录不足以用于本地开发，则可通过AEM SDK的本地快速启动日志支持Web控制台（位于）([/system/console/slinglog](http://localhost:4502/system/console/slinglog))配置临时日志记录，但不建议将临时更改持续到Git，除非在AEM作为Cloud Service开发环境也需要这些相同的日志配置。 请记住，通过日志支持控制台所做的更改会直接保留到AEM SDK的本地快速启动存储库。

Java日志语句可以是`error.log`文件中的视图:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，将输出流化到终端的`error.log`“尾部”是有用的。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要[第三方尾部应用程序](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)或使用[Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## 调度程序日志

调用`bin/docker_run`时，调度程序日志将输出到stdout，但可以直接访问Docker包含的日志。

### 访问Docker容器{#dispatcher-tools-access-logs}中的日志

调度程序日志可以直接访问位于`/etc/httpd/logs`的Docker容器。

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

_必 `<CONTAINER ID>` 须 `docker exec -it <CONTAINER ID> /bin/sh` 用命令中列出的目标Docker容器ID替换 `docker ps` 中。_


### 将Docker日志复制到本地文件系统{#dispatcher-tools-copy-logs}

调度程序日志可以从位于`/etc/httpd/logs`的Docker容器中复制到本地文件系统，以便使用您喜爱的日志分析工具进行检查。 请注意，这是一个时间点拷贝，不提供日志的实时更新。

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

_必 `<CONTAINER_ID>` 须 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 用命令中列出的目标Docker容器ID替换 `docker ps` 中。_
