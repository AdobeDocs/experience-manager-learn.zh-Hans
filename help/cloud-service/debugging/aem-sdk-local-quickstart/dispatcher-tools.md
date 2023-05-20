---
title: 偵錯Dispatcher工具
description: Dispatcher工具提供容器化的Apache Web Server環境，可用於在本機模擬AEM作為Cloud Services的AEM Publish服務的Dispatcher。 偵錯Dispatcher工具的記錄和快取內容對於確保端對端AEM應用程式以及支援的快取和安全設定正確無誤至關重要。
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

# 偵錯Dispatcher工具

Dispatcher工具提供容器化的Apache Web Server環境，可用於在本機模擬AEM作為Cloud Services的AEM Publish服務的Dispatcher。

偵錯Dispatcher工具的記錄和快取內容對於確保端對端AEM應用程式以及支援的快取和安全設定正確無誤至關重要。

>[!NOTE]
>
>由於Dispatcher工具是以容器為基礎，每次重新啟動時，先前的記錄和快取內容都會遭到破壞。

## Dispatcher工具記錄

Dispatcher工具記錄可透過 `stdout` 或 `bin/docker_run` 命令或提供更多詳細資訊，可在Docker容器中找到，網址為 `/etc/https/logs`.

另請參閱 [Dispatcher記錄](./logs.md#dispatcher-logs) 有關如何直接存取Dispatcher工具的Docker容器記錄檔的說明。

## Dispatcher工具快取

### 存取Docker容器中的日誌

Dispatcher快取可以直接在Docker容器中存取，網址為 ` /mnt/var/www/html`.

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

### 將Docker日誌複製到本機檔案系統

可以從Docker容器複製排程程式日誌，位置為 `/mnt/var/www/html` 至本機檔案系統以使用您最喜愛的工具進行檢查。 請注意，這是時間點復本，不會提供快取的即時更新。

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
