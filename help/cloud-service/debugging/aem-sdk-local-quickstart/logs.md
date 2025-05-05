---
title: 使用日志调试AEM SDK
description: 日志是调试AEM应用程序的首选工具，但需要取决于已部署的AEM应用程序中是否有足够的日志记录。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# 使用日志调试AEM SDK

访问AEM SDK的日志， AEM SDK本地快速入门Jar或Dispatcher工具可以提供有关调试AEM应用程序的重要见解。

## AEM日志

>[!VIDEO](https://video.tv.adobe.com/v/38143?quality=12&learn=on&captions=chi_hans)

日志是调试AEM应用程序的首选工具，但需要取决于已部署的AEM应用程序中是否有足够的日志记录。 Adobe建议尽量保持本地开发和AEM as a Cloud Service开发日志记录配置相似，因为它规范了AEM SDK的本地快速启动和AEM as a Cloud Service开发环境的日志可见性，从而减少了配置扭曲和重新部署。

[AEMAEM项目原型](https://github.com/adobe/aem-project-archetype)通过Sling Logger OSGi配置(位于

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

登录到`error.log`。

如果默认日志记录不足以进行本地开发，则可以通过AEM SDK的本地快速入门的日志支持Web控制台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))配置临时日志记录，但不建议将临时更改保留到Git中，除非也在AEM as a Cloud Service开发环境中需要这些相同的日志配置。 请记住，通过日志支持控制台进行的更改将直接保留到AEM SDK的本地快速入门存储库。

可以在`error.log`文件中查看Java日志语句：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常，“跟踪”将其输出流式传输到终端的`error.log`很有用。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要[第三方尾部应用程序](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command)或使用[Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936)。

## Dispatcher日志

调用`bin/docker_run`时，Dispatcher日志将输出到stdout，不过，可以通过Docker包含中的直接访问日志。

### 访问Docker容器中的日志{#dispatcher-tools-access-logs}

Dispatcher日志可以直接访问位于`/etc/httpd/logs`的Docker容器中。

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

_`docker exec -it <CONTAINER ID> /bin/sh`中的`<CONTAINER ID>`必须替换为从`docker ps`命令中列出的目标Docker容器ID。_


### 将Docker日志复制到本地文件系统{#dispatcher-tools-copy-logs}

可以使用您喜爱的日志分析工具将Dispatcher日志从`/etc/httpd/logs`处的Docker容器复制到本地文件系统以进行检查。 请注意，这是一个时间点副本，不提供日志的实时更新。

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

_`docker cp <CONTAINER_ID>:/var/log/apache2 ./`中的`<CONTAINER_ID>`必须替换为从`docker ps`命令中列出的目标Docker容器ID。_
