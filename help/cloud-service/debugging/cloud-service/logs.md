---
title: 日志
description: 記錄檔是在AEMas a Cloud Service中偵錯AEM應用程式的前線，但部署的AEM應用程式中必須有充足的登入次數。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 2%

---

# 使用記錄檔對AEMas a Cloud Service除錯

記錄檔是在AEMas a Cloud Service中偵錯AEM應用程式的前線，但部署的AEM應用程式中必須有充足的登入次數。

指定環境的AEM服務（作者、發佈/發佈Dispatcher）的所有記錄活動都會合併為單一記錄檔案，即使該服務中的不同pod會產生記錄陳述式亦然。

每個log陳述式中都會提供Pod ID，並允許篩選或整理log陳述式。 Pod Id的格式為：

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ 示例: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## 自訂記錄檔

AEM as a Cloud Services不支援自訂記錄檔，但是它支援自訂記錄。

若要在AEMas a Cloud Service中使用Java記錄(透過 [Cloud Manager](#cloud-manager) 或 [ADOBE I/OCLI](#aio))，自訂記錄陳述式必須寫入 `error.log`. 寫入自訂具名記錄檔的記錄檔，例如 `example.log`無法從AEMas a Cloud Service存取。

記錄檔可以寫入 `error.log` 在應用程式的「 」中使用Sling LogManager OSGi設定屬性 `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` 檔案。

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM作者和發佈服務記錄

AEM Author和Publish服務都提供AEM執行階段伺服器記錄檔：

+ `aemerror` 是Java錯誤記錄(位於 `/crx-quickstart/logs/error.log` (在AEM SDK本機Quickstart上)。 以下為 [建議的記錄層級](#log-levels) 對於每種環境型別的自訂記錄器：
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemaccess` 列出對AEM服務的HTTP要求與詳細資訊
+ `aemrequest` 列出向AEM服務提出的HTTP要求及其對應的HTTP回應

## AEM發佈Dispatcher記錄

只有AEM Publish Dispatcher會提供Apache Web Server和Dispatcher記錄檔，因為這些方面僅存在於AEM Publish層級，而不存在於AEM Author層級。

+ `httpdaccess` 列出向AEM服務的Apache Web Server/Dispatcher提出的HTTP請求。
+ `httperror`  列出來自Apache Web Server的記錄訊息，以及偵錯受支援的Apache模組(例如 `mod_rewrite`.
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`
+ `aemdispatcher` 列出來自Dispatcher模組的記錄訊息，包括從快取訊息篩選和提供服務。
   + 开发: `DEBUG`
   + 暂存: `WARN`
   + 生产: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager可讓您透過環境的下載記錄檔動作，依日下載記錄檔。

![Cloud Manager — 下載記錄](./assets/logs/download-logs.png)

您可以透過任何記錄分析工具下載及檢查這些記錄。

## 使用Cloud Manager外掛程式Adobe I/OCLI{#aio}

Adobe Cloud Manager支援透過存取AEMas a Cloud Service記錄 [ADOBE I/OCLI](https://github.com/adobe/aio-cli) 使用 [Adobe I/OCLI的Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager).

首先， [使用Cloud Manager外掛程式設定Adobe I/O](../../local-development-environment/development-tools.md#aio-cli).

確保已識別相關的計畫ID和環境ID，並使用 [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) 以列出用於 [尾部](#aio-cli-tail-logs) 或 [下載](#aio-cli-download-logs) 記錄。

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### 追蹤記錄{#aio-cli-tail-logs}

Adobe I/OCLI能夠使用AEMas a Cloud Service即時追蹤記錄檔 [尾部日誌](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) 命令。 在AEMas a Cloud Service環境中執行動作時，追蹤對於監視即時記錄活動很有用。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

其他命令列工具，例如 `grep` 可搭配使用 `tail-logs` 協助隔離感興趣的記錄陳述式，例如：

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

...僅顯示產生的記錄陳述式 `com.example.MySlingModel` 或包含該字串。

### 正在下載記錄檔{#aio-cli-download-logs}

Adobe I/OCLI可讓您使用從AEMas a Cloud Service下載記錄檔 [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days))命令。 這將提供與從Cloud Manager網頁UI下載記錄檔相同的最終結果，差異為 `download-logs` 命令會根據要求的記錄天數，整合成天內的記錄。

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## 瞭解記錄

AEMas a Cloud Service中的記錄有多個Pod可將記錄陳述式寫入其中。 由於多個AEM執行個體會寫入相同的記錄檔，因此請務必瞭解如何分析並減少偵錯時的雜訊。 若要說明，請執行下列動作 `aemerror` 使用的記錄檔片段：

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

使用Pod Id （日期和時間之後的資料點）時，記錄檔可由Pod或服務內的AEM例項整理，更易於追蹤和瞭解程式碼執行。

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## 建議的記錄層級{#log-levels}

Adobe對於每個AEMas a Cloud Service環境的記錄層級的一般指引如下：

+ 本機開發(AEM SDK)： `DEBUG`
+ 开发: `DEBUG`
+ 暂存: `WARN`
+ 生产: `ERROR`

為每種環境型別設定最適當的記錄層級是使用AEMas a Cloud Service，記錄層級會保留在程式碼中

+ OSGi設定中會維護Java記錄設定
+ Dispatcher專案中的Apache網頁伺服器和Dispatcher記錄層級

...因此需要部署才能變更。

### 用於設定Java記錄層級的環境特定變數

為每個環境設定靜態且眾所周知的Java記錄層級的替代方法，是使用AEM作為Cloud Service [環境特定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) 將記錄層級引數化，允許值透過 [使用Cloud Manager外掛程式Adobe I/OCLI](#aio-cli).

這需要更新記錄OSGi設定，以使用環境特定的變數預留位置。 [預設值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) （記錄層級）的設定方式應為 [Adobe建議](#log-levels). 例如：

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

此方法的缺點必須列入考量：

+ [允許有限數量的環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)，而建立變數以管理記錄層級將使用一個。
+ 環境變數只能透過以下方式以程式設計方式管理 [ADOBE I/OCLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) 或 [Cloud Manager HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ 環境變數的變更必須由支援的工具手動重設。 若忘記將高流量環境（例如生產）重設為較不詳細的記錄層級，可能會淹沒記錄檔並影響AEM效能。

_環境特定變數無法用於Apache Web Server或Dispatcher記錄設定，因為這些設定未透過OSGi設定。_
