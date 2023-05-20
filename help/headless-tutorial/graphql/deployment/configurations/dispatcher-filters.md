---
title: 適用於AEM GraphQL的Dispatcher篩選器
description: 瞭解如何設定AEM Publish Dispatcher篩選器以與AEM GraphQL搭配使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---

# Dispatcher篩選器

Adobe Experience Manager as a Cloud Service會使用AEM發佈Dispatcher篩選器，以確保只有可聯絡AEM的請求才能聯絡AEM。 預設會拒絕所有要求，而且必須明確新增允許URL的模式。

| 使用者端型別 | [單頁應用程式(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [移动设备](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher篩選器設定 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下設定為範例。 請確定您調整這些值，以符合專案的要求。

## Dispatcher篩選設定

AEM發佈Dispatcher篩選設定會定義允許到達AEM的URL模式，且必須包含AEM持續查詢端點的URL首碼。

| 使用者端連線至 | AEM Author | AEM 发布 | AEM預覽 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher篩選器設定 | ✘ | ✔ | ✔ |

新增 `allow` 具有URL模式的規則 `/graphql/execute.json/*`，並確保檔案ID (例如 `/0600`，在範例伺服器陣列檔案中是唯一的)。
這可讓持續查詢端點收到HTTPGET要求，例如 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` 到AEM Publish。

如果您在AEM Headless體驗中使用體驗片段，請對這些路徑執行相同的操作。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### 篩選設定範例

+ [WKND專案中可以找到Dispatcher篩選的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
