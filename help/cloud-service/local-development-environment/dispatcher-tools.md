---
title: 為AEMas a Cloud Service開發設定Dispatcher工具
description: AEM SDK的Dispatcher工具可讓您在本機輕鬆安裝、執行及疑難排解Dispatcher，促進Adobe Experience Manager (AEM)專案的本機開發。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 9%

---

# 設定本機Dispatcher工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本地 Dispatcher 工具"
>abstract="Dispatcher 是整个 Experience Manager 架构的组成部分，应该是本地开发设置的一部分。AEM as a Cloud Service SDK 包括推荐的 Dispatcher 工具版本，该版本有助于在本地配置、验证和模拟 Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="云中的 Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下载 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM)的Dispatcher是Apache HTTP Web伺服器模組，在CDN和AEM Publish層級之間提供安全和效能層。 Dispatcher 是整个 Experience Manager 架构的组成部分，应该是本地开发设置的一部分。

AEM as a Cloud Service SDK 包括推荐的 Dispatcher 工具版本，该版本有助于在本地配置、验证和模拟 Dispatcher。Dispatcher工具由以下部分組成：

+ Apache HTTP Web伺服器和Dispatcher設定檔案的基準集合，位於 `.../dispatcher-sdk-x.x.x/src`
+ 組態驗證器CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validate`
+ 組態產生CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 組態部署CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 不可變的組態檔會覆寫CLI工具，位於 `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ 使用Dispatcher模組執行Apache HTTP Web伺服器的Docker影像

請注意 `~` 用作使用者目錄的簡寫。 在Windows中，這相當於 `%HOMEPATH%`.

>[!NOTE]
>
> 此頁面中的影片是錄製在macOS上。 Windows使用者可以跟著執行，但使用每個視訊提供的對等Dispatcher工具Windows命令。

## 前提条件

1. Windows使用者必須使用Windows 10專業版（或支援Docker的版本）
1. 安裝 [Experience Manager發佈快速入門Jar](./aem-runtime.md) 在本機開發機器上。

+ 可選擇安裝最新的 [AEM參考網站](https://github.com/adobe/aem-guides-wknd/releases) 在本機AEM Publish服務上。 本教學課程會使用此網站將運作中的Dispatcher視覺化。

1. 安裝並啟動最新版的 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)本機開發電腦上。

## 下載Dispatcher工具(做為AEM SDK的一部分)

AEMas a Cloud ServiceSDK (或AEM SDK)包含用於執行Apache HTTP Web伺服器的Dispatcher工具（搭配用於開發的Dispatcher模組在本機）以及相容的QuickStart Jar。

如果AEMas a Cloud ServiceSDK已下載至 [設定本機AEM執行階段](./aem-runtime.md)，則不需要重新下載。

1. 登入 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) 使用您的Adobe ID
   + 您的Adobe組織 __必須__ 布建AEMas a Cloud Service以下載AEMas a Cloud ServiceSDK
1. 按一下最新的 __AEM SDK__ 要下載的結果列

## 從AEM SDK zip解壓縮Dispatcher工具

>[!TIP]
>
> Windows使用者在包含本機Dispatcher工具的資料夾的路徑中不能有任何空格或特殊字元。 如果路徑中存在空格， `docker_run.cmd` 失敗。

Dispatcher工具的版本與AEM SDK的版本不同。 確保透過符合AEMas a Cloud Service版本的AEM SDK版本提供Dispatcher工具的版本。

1. 將下載的解壓縮 `aem-sdk-xxx.zip` 檔案
1. 將Dispatcher工具解壓縮到 `~/aem-sdk/dispatcher`

+ Windows：解壓縮 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 到 `C:\Users\<My User>\aem-sdk\dispatcher` （視需要建立遺失的資料夾）
+ macOS Linux®：執行隨附的Shell指令碼 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 解壓縮Dispatcher工具的方式
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

以下發出的所有命令都假設目前的工作目錄包含展開的Dispatcher工具內容。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*本影片使用macOS作說明用途。 可使用等同的Windows/Linux指令來取得類似的結果。*

## 瞭解Dispatcher設定檔案

>[!TIP]
> Experience Manager從建立的專案 [AEM專案Maven原型](https://github.com/adobe/aem-project-archetype) 會預先填入這組Dispatcher設定檔案，因此不需要從Dispatcher工具src資料夾進行複製。

Dispatcher工具提供了一組Apache HTTP Web伺服器和Dispatcher設定檔案，用於定義所有環境（包括本機開發）的行為。

這些檔案旨在複製到Experience ManagerMaven專案的 `dispatcher/src` 資料夾(如果它們尚未存在於Experience ManagerMaven專案中)。

解壓縮的Dispatcher工具中提供了配置檔案的完整說明，如下所示 `dispatcher-sdk-x.x.x/docs/Config.html`.

## 驗證設定

Dispatcher和Apache Web伺服器設定(可選，透過 `httpd -t`)可使用進行驗證 `validate` 指令碼(不要與 `validator` 可執行檔)。 此 `validate` script提供執行 [三個階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) 的 `validator`.

+ 用途:
   + Windows: `bin\validate src`
   + macOS Linux®： `./bin/validate.sh ./src`

## 在本機執行Dispatcher

AEM Dispatcher是使用Docker在本機針對 `src` Dispatcher和Apache Web Server設定檔案。

+ 用途:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®： `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

此 `<aem-publish-host>` 可設為 `host.docker.internal`，Docker在容器中提供的特殊DNS名稱，可解析成主機電腦的IP。 如果 `host.docker.internal` 無法解析，請參閱 [疑難排解](#troubleshooting-host-docker-internal) 區段底下。

例如，若要使用Dispatcher工具提供的預設設定檔案啟動Dispatcher Docker容器：

啟動Dispatcher Docker容器，提供Dispatcher設定src資料夾的路徑：

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®： `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

AEMas a Cloud ServiceSDK的發佈服務會在連線埠4503上本機執行，可透過Dispatcher存取 `http://localhost:8080`.

若要針對Experience Manager專案的Dispatcher設定執行Dispatcher工具，請指向您專案的 `dispatcher/src` 資料夾。

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®：

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher工具記錄

Dispatcher記錄在本機開發期間有助於瞭解HTTP請求是否遭到封鎖及其原因。 記錄層級可藉由在執行前置詞來設定 `docker_run` 環境引數時。

Dispatcher工具記錄檔在下列情況下發出到標準 `docker_run` 執行前填入。

用於偵錯Dispatcher的有用引數包括：

+ `DISP_LOG_LEVEL=Debug` 將Dispatcher模組記錄設定為偵錯層級
   + 默认值为: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 將Apache HTTP Web伺服器重寫模組記錄設定為偵錯層級
   + 默认值为: `Warn`
+ `DISP_RUN_MODE` 設定Dispatcher環境的「執行模式」，載入對應的執行模式Dispatcher設定檔案。
   + 默认为 `dev`
+ 有效值： `dev`， `stage`，或 `prod`

一或多個引數可以傳遞至 `docker_run`

+ Windows:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®：

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### 記錄檔存取

可直接在Docker容器中存取Apache Web Server和AEM Dispatcher記錄：

+ [存取Docker容器中的日誌](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [將Docker日誌複製到本機檔案系統](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何時更新Dispatcher工具{#dispatcher-tools-version}

Dispatcher工具的版本增加頻率低於Experience Manager，因此Dispatcher工具在本機開發環境中需要的更新較少。

建議的Dispatcher工具版本為搭配AEMas a Cloud ServiceSDK使用，且符合Experience Manageras a Cloud Service版本。 AEMas a Cloud Service版本可透過以下方式找到： [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager >環境__，依指定的環境而定 __AEM版本__ 標籤

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

*請注意，Dispatcher工具版本與Experience Manager版本不符。*

## 如何更新Apache和Dispatcher設定的基準線集

Apache和Dispatcher設定的基準集已定期增強，並隨AEMas a Cloud ServiceSDK版本發行。 最佳實務是將基準線設定增強功能併入您的AEM專案，並避免 [本機驗證](#validate-configurations) 和Cloud Manager管道故障。 使用更新它們 `update_maven.sh` 指令碼來自 `.../dispatcher-sdk-x.x.x/bin` 資料夾。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*本影片使用macOS作說明用途。 可使用等同的Windows/Linux指令來取得類似的結果。*


假設您過去曾使用建立AEM專案 [AEM專案原型](https://github.com/adobe/aem-project-archetype)，基準Apache和Dispatcher設定為最新。 使用這些基準線設定，您的專案特定設定是透過重複使用和複製檔案 `*.vhost`， `*.conf`， `*.farm` 和 `*.any` 從 `dispatcher/src/conf.d` 和 `dispatcher/src/conf.dispatcher.d` 資料夾。 您的本機Dispatcher驗證和Cloud Manager管道正常運作。

同時，由於新功能、安全性修正和最佳化等多種原因，基準Apache和Dispatcher設定已得到增強。 它們透過較新版本的Dispatcher工具發行，作為AEMas a Cloud Service發行版本的一部分。

現在，針對最新的Dispatcher工具版本驗證您的專案特定Dispatcher設定時，它們會開始失敗。 若要解決此問題，需使用下列步驟更新基準線設定：

+ 驗證針對最新Dispatcher工具版本驗證是否失敗

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ 使用更新不可變檔案 `update_maven.sh` 指令碼

   ```shell
   $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Updating dispatcher configuration at folder 
   running in 'extract' mode
   running in 'extract' mode
   reading immutable file list from /etc/httpd/immutable.files.txt
   preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
   ...
   immutable files extraction COMPLETE
   fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
   Cloud manager validator 2.0.53
   ```

+ 驗證更新的不可變檔案，例如 `dispatcher_vhost.conf`， `default.vhost`、和 `default.farm` 如有需要，請在自訂檔案中進行衍生自這些檔案的相關變更。

+ 重新驗證設定，應該會通過

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ 本機驗證變更後，認可更新的設定檔案

## 疑难解答

### docker_run導致「等到host.docker.internal可用」訊息{#troubleshooting-host-docker-internal}

此 `host.docker.internal` 是提供給Docker包含並解析到主機的主機名稱。 根據docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/)， [Windows](https://docs.docker.com/desktop/networking/))：

> 從Docker 18.03開始，建議連線到特殊的DNS名稱host.docker.internal，這會解析為主機使用的內部IP位址

時間 `bin/docker_run src host.docker.internal:4503 8080` 結果中的訊息 __等待host.docker.internal可用__，然後：

1. 確保安裝的Docker版本為18.03或更高版本
2. 您可能設定了本機電腦，無法註冊/解析 `host.docker.internal` 名稱。 請改用您的本機IP。
   + Windows:
   + 在命令提示字元中，執行 `ipconfig`，並記錄主機的 __Ipv4位址__ 主機的。
   + 然後，執行 `docker_run` 使用此IP位址：
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®：
   + 從終端機，執行 `ifconfig` 並記錄主機 __入門__ IP位址，通常是 __en0__ 裝置。
   + 然後執行 `docker_run` 使用主機IP位址：
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 範例錯誤

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## 其他资源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [下載AEM參考網站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience ManagerDispatcher檔案](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
