---
title: 使用日志调试AEM SDK
description: 日志是调试AEM应用程序的首选工具，但取决于已部署的AEM应用程序中是否有足够的日志记录。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# 使用日志调试AEM SDK

访问AEM SDK的日志，AEM SDK本地快速入门Jar或Dispatcher工具可以提供有关调试AEM应用程序的关键分析。

## AEM日志

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

日志是调试AEM应用程序的首选工具，但取决于已部署的AEM应用程序中是否有足够的日志记录。 Adobe建议将本地开发和AEM作为Cloud Service开发日志记录配置保持尽可能相似，因为它可将日志在AEM SDK的本地快速启动和AEM作为Cloud Service的开发环境中的可见性标准化，从而减少配置篡改和重新部署。

[AEM项目原型](https://github.com/adobe/aem-project-archetype)通过位于以下位置的Sling Logger OSGi配置，为AEM应用程序的Java包在DEBUG级别配置日志记录以用于本地开发

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

日志到`error.log`。

如果默认日志记录不足以用于本地开发，则可以通过AEM SDK的本地快速启动日志支持Web控制台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))配置临时日志记录，但不建议将临时更改持久保留到Git，除非作为Cloud Service开发环境，AEM上也需要这些相同的日志配置。 请记住，通过日志支持控制台所做的更改将直接保留到AEM SDK的本地快速启动存储库。

可以在`error.log`文件中查看Java日志语句：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，将输出流传输到终端的`error.log`“跟踪”会很有用。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要[第三方尾应用程序](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)或使用[Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## 调度程序日志

调用`bin/docker_run`时，调度程序日志将输出到stdout，但Docker包含的中可以直接通过访问日志。

### 访问Docker容器中的日志{#dispatcher-tools-access-logs}

调度程序日志可以直接在位于`/etc/httpd/logs`的Docker容器中访问。

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

_中 `<CONTAINER ID>` 的必 `docker exec -it <CONTAINER ID> /bin/sh` 须替换为命令中列出的目标Docker容器 `docker ps` ID。_


### 将Docker日志复制到本地文件系统{#dispatcher-tools-copy-logs}

调度程序日志可以从位于`/etc/httpd/logs`的Docker容器复制到本地文件系统，以便使用您喜爱的日志分析工具进行检查。 请注意，这是一个时间点副本，不会实时更新日志。

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

_中 `<CONTAINER_ID>` 的必 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 须替换为命令中列出的目标Docker容器 `docker ps` ID。_
