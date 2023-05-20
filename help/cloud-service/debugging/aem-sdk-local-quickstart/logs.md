---
title: 使用記錄檔偵錯AEM SDK
description: 記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須有充足的登入次數。
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

# 使用記錄檔偵錯AEM SDK

存取AEM SDK的記錄(AEM SDK本機快速入門Jar或Dispatcher工具)可以提供AEM應用程式偵錯的重要深入分析。

## AEM記錄

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

記錄檔是AEM應用程式偵錯作業的最前線，但部署的AEM應用程式必須有充足的登入次數。 Adobe建議儘可能保持本機開發和AEMas a Cloud Service開發記錄設定，因為它在AEM SDK的本機Quickstart和AEMas a Cloud Service的開發環境中將記錄可見性標準化，並減少設定迂迴和重新部署。

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) AEM透過Sling Logger OSGi設定(位於

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

哪些記錄到 `error.log`.

如果預設記錄不足以用於本機開發，則可透過AEM SDK的本機Quickstart的Log Support Web主控台([/system/console/slinglog](http://localhost:4502/system/console/slinglog))，不過不建議將臨機變更保留至Git，除非AEMas a Cloud Service開發環境也需要相同的記錄設定。 請記住，透過記錄支援主控台的變更會直接儲存至AEM SDK的本機Quickstart存放庫。

Java記錄陳述式可以在 `error.log` 檔案：

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

通常「追蹤」以下專案會很實用： `error.log` 將其輸出串流到終端機。

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows需要 [第三方尾部應用程式](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) 或使用 [Powershell的Get-Content命令](https://stackoverflow.com/a/46444596/133936).

## Dispatcher記錄

Dispatcher記錄會輸出到stdout，當 `bin/docker_run` 會叫用，但可直接透過在Docker包含中存取記錄。

### 存取Docker容器中的日誌{#dispatcher-tools-access-logs}

Dispatcher記錄可直接存取Docker容器中的 `/etc/httpd/logs`.

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

_此 `<CONTAINER ID>` 在 `docker exec -it <CONTAINER ID> /bin/sh` 必須替換為以下清單中的目標Docker容器ID： `docker ps` 命令。_


### 將Docker日誌複製到本機檔案系統{#dispatcher-tools-copy-logs}

可以從Docker容器複製排程程式日誌，位置為 `/etc/httpd/logs` 至本機檔案系統以使用您最喜愛的記錄分析工具進行檢查。 請注意，這是時間點復本，不會提供記錄檔的即時更新。

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

_此 `<CONTAINER_ID>` 在 `docker cp <CONTAINER_ID>:/var/log/apache2 ./` 必須替換為以下清單中的目標Docker容器ID： `docker ps` 命令。_
