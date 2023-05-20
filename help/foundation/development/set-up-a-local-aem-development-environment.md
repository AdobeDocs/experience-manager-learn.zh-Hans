---
title: 設定本機AEM開發環境
description: 瞭解如何設定本機開發環境以進行Experience Manager。 熟悉本機安裝、Apache Maven、整合式開發環境，以及偵錯和疑難排解。 使用Eclipse IDE、CRXDE-Lite、Visual Studio Code和IntelliJ。
version: 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
thumbnail: aem-local-dev-env.jpg
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '2603'
ht-degree: 1%

---

# 設定本機AEM開發環境

為Adobe Experience Manager、AEM設定本機開發的指南。 涵蓋本機安裝、Apache Maven、整合式開發環境和偵錯/疑難排解等重要主題。 開發工具 **Eclipse IDE、CRXDE Lite、Visual Studio Code和IntelliJ** 將會進行討論。

## 概述

為Adobe Experience Manager或AEM進行開發時，第一步是設定本機開發環境。 請花點時間設定高品質的開發環境，以提升生產力並更快撰寫更好的程式碼。 我們可以將AEM本機開發環境分成四個區域：

* 本機AEM執行個體
* [!DNL Apache Maven] 專案
* 整合式開發環境(IDE)
* 疑难解答

## 安裝本機AEM執行個體

當我們提及本機AEM執行個體時，我們談論的是在開發人員個人電腦上執行的Adobe Experience Manager復本。 ***全部*** AEM開發應該從撰寫和執行本機AEM執行個體的程式碼開始。

如果您是AEM的新手，可以安裝兩種基本執行模式： ***作者*** 和 ***發佈***. 此 ***作者*** [執行模式](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  是數位行銷人員用來建立和管理內容的環境。 大部分時間進行開發時，您是將程式碼部署至Author例項。 這可讓您建立頁面，以及新增和設定元件。 AEM Sites是WYSIWYG編寫CMS，因此大部分CSS和JavaScript都可透過編寫執行個體進行測試。

它也是 *關鍵* 針對本機測試程式碼 ***發佈*** 執行個體。 此 ***發佈*** 執行個體是您網站的訪客與之互動的AEM環境。 而 ***發佈*** 執行個體與的技術棧疊相同 ***作者*** 執行個體而言，設定和許可權有一些重大差異。 程式碼必須針對本機進行測試 ***發佈*** 執行個體，然後再提升至較高層級的環境。

### 步骤

1. 確認已安裝Java™。
   * 偏好 [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 適用於AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) 適用於AEM 6.5之前的AEM版本
1. 取得 [AEM QuickStart Jar和 [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html).
1. 在電腦上建立檔案夾結構，如下所示：

```plain
~/aem-sdk
    /author
    /publish
```

1. 重新命名 [!DNL QuickStart] JAR至 ***aem-author-p4502.jar*** 並將其放在 `/author` 目錄。 新增 ***[!DNL license.properties]*** 檔案於 `/author` 目錄。

1. 複製 [!DNL QuickStart] JAR，將其重新命名為 ***aem-publish-p4503.jar*** 並將其放在 `/publish` 目錄。 新增 ***[!DNL license.properties]*** 檔案於 `/publish` 目錄。

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. 連按兩下 ***aem-author-p4502.jar*** 檔案以安裝 **作者** 執行個體。 這會啟動編寫執行個體（在連線埠上執行） **4502** 本機電腦上。

連按兩下 ***aem-publish-p4503.jar*** 檔案以安裝 **發佈** 執行個體。 這會啟動在連線埠上執行的發佈執行個體 **4503** 本機電腦上。

>[!NOTE]
>
>視您的開發機器硬體而定，可能很難同時擁有 **製作與發佈** 執行個體同時執行。 您很少需要在本機設定上同時執行這兩項。

### 使用命令列

連按兩下JAR檔案的替代方法是，從命令列啟動AEM或建立指令碼(`.bat` 或 `.sh`)視您當地的作業系統風格而定。 以下是範例指令的範例：

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

在此， `-X` 是JVM選項和 `-D` 是其他框架屬性，如需詳細資訊，請參閱 [部署和維護AEM執行個體](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) 和 [快速入門檔案中可用的其他選項](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## 安裝Apache Maven

***[!DNL Apache Maven]*** 是一種用於管理Java型專案的建置和部署程式的工具。 AEM是以Java為基礎的平台，並且 [!DNL Maven] 是管理AEM專案程式碼的標準方式。 當我們說 ***AEM Maven專案*** 或只是您的 ***AEM專案***，我們指的是一個Maven專案，其中包含所有 *自訂* 您網站的程式碼。

所有AEM專案都應建置在最新版本的 **[!DNL AEM Project Archetype]**： [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). 此 [!DNL AEM Project Archetype] 提供AEM專案的啟動程式，其中包含一些範常式式碼和內容。 此 [!DNL AEM Project Archetype] 也包含 **[!DNL AEM WCM Core Components]** 已設定為用於您的專案。

>[!CAUTION]
>
>開始新專案時，最佳實務是使用原型的最新版本。 請記住，原型有多個版本，並非所有版本都與舊版AEM相容。

### 步骤

1. 下載 [Apache Maven](https://maven.apache.org/download.cgi)
2. 安裝 [Apache Maven](https://maven.apache.org/install.html) 並確保已將安裝新增至命令列 `PATH`.
   * [!DNL macOS] 使用者可以使用以下工具安裝Maven： [Homebrew](https://brew.sh/)
3. 確認 **[!DNL Maven]** 是透過開啟新的命令列終端機並執行以下命令來安裝：

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> 在中，過去新增的 `adobe-public` 需要Maven設定檔才能指向 `nexus.adobe.com` 以下載AEM成品。 AEM現在您可以透過Maven Central和 `adobe-public` 不需要設定檔。

## 設定整合式開發環境

整合式開發環境或IDE是一種結合文字編輯器、語法支援和建置工具的應用程式。 根據您正在執行的開發型別，一個IDE可能比另一個IDE更適合。 不論IDE為何，定期進行 ***推播*** 編寫本機AEM例項的程式碼以進行測試。 請務必偶爾 ***提取*** 從本機AEM執行個體到您的AEM專案的設定，以便持續存在到Git等原始檔控制管理系統。

以下是一些與AEM開發搭配使用的較熱門IDE，以及顯示與本機AEM執行個體整合的對應影片。

>[!NOTE]
>
> WKND專案已更新為預設可在AEMas a Cloud Service上運作。 已更新為 [向後相容於6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

當使用IDE時，請確保檢查 `classic` 在您的Maven設定檔索引標籤中。

![Maven設定檔標籤](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven設定檔*

### [!DNL Eclipse] IDE

此 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 是Java™開發中較常用的IDE之一，大部分是因為它是開放原始碼 ***免費***！ Adobe提供外掛程式， **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**，代表 [!DNL Eclipse] 以使用良好的GUI更輕鬆地進行開發，將程式碼與本機AEM執行個體同步。 此 [!DNL Eclipse] IDE建議不熟悉AEM的開發人員使用，主要是因為GUI支援來自 [!DNL AEM Developer Tools].

#### 安裝及設定

1. 下載並安裝 [!DNL Eclipse] IDE用於 [!DNL Java™ EE Developers]： [https://www.eclipse.org](https://www.eclipse.org/)
1. 依照指示安裝 [!DNL AEM Developer Tools] 外掛程式： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 匯入Maven專案
* 01:24 — 使用Maven建置和部署原始程式碼
* 04:33 — 使用AEM開發人員工具推送程式碼變更
* 10:55 — 使用AEM Developer Tool提取程式碼變更
* 13:12 — 使用Eclipse的整合式偵錯工具

### IntelliJ IDEA

此 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 是適用於專業Java™開發的強大IDE。 [!DNL IntelliJ IDEA] 有兩種口味，a ***免費*** [!DNL Community] 版本和商業（付費） [!DNL Ultimate] 版本。 免費 [!DNL Community] 版本 [!DNL IntellIJ IDEA] 足以進行更多AEM開發，但是 [!DNL Ultimate] [擴充其功能集](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. 下載並安裝 [!DNL IntelliJ IDEA]： [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安裝 [!DNL Repo] （命令列工具）： [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 — 匯入Maven專案
* 05:47 — 使用Maven建置和部署原始程式碼
* 08:17 — 使用存放庫推送變更
* 14:39 — 使用存放庫提取變更
* 17:25 — 使用IntelliJ IDEA的整合式偵錯工具

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** 已迅速成為最愛的工具 ***前端開發人員*** 加上增強的JavaScript支援， [!DNL Intellisense]和瀏覽器偵錯支援。 **[!DNL Visual Studio Code]** 是開放原始碼、免費，並具備許多強大的擴充功能。 [!DNL Visual Studio Code] 可設定為在Adobe工具的協助下與AEM整合， **[存放庫](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** 您也可以安裝數個社群支援的擴充功能，以便與AEM整合。

[!DNL Visual Studio Code] 對於主要撰寫CSS/LESS和JavaScript程式碼以建立AEM使用者端程式庫的前端開發人員而言，這是一個絕佳的選擇。 此工具可能不是新AEM開發人員的最佳選擇，因為節點定義（對話方塊、元件）需要以原始XML編輯。 有數個Java™擴充功能可用於 [!DNL Visual Studio Code]但如果主要執行Java™開發 [!DNL Eclipse IDE] 或 [!DNL IntelliJ] 可能偏好使用。

#### 重要連結

* [**下載**](https://code.visualstudio.com/Download) **Visual Studio Code**
* **[存放庫](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - JCR內容的類似FTP工具
* **[已棄用](https://aemfed.io/)**  — 加速您的AEM前端工作流程
* **[AEM同步](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  — 社群支援&#42; Visual Studio Code的擴充功能

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 匯入Maven專案
* 00:53 — 使用Maven建置和部署原始程式碼
* 04:03 — 使用存放庫命令列工具推送程式碼變更
* 08:29 — 使用Repo命令列工具提取程式碼變更
* 10:40 — 使用嵌入式工具推送程式碼變更
* 14:24 — 疑難排解，重建使用者端程式庫

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) 是AEM存放庫的瀏覽器式檢視。 [!DNL CRXDE Lite] 內嵌於AEM中，可讓開發人員執行標準開發任務，例如編輯檔案、定義元件、對話方塊和範本。 [!DNL CRXDE Lite] 是 ***not*** 旨在作為完整開發環境，但可有效作為偵錯工具。 [!DNL CRXDE Lite] 在延伸或只是瞭解程式碼庫以外的產品程式碼時，這很有用。 [!DNL CRXDE Lite] 提供存放庫的強大檢視，以及有效測試和管理許可權的方法。

[!DNL CRXDE Lite] 應該搭配其他IDE使用來測試和偵錯程式碼，但絕不能作為主要開發工具。 其語法支援有限、沒有自動完成功能，而且與原始檔控制管理系統的整合也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的程式碼無法運作！ 就像所有開發一樣，有時候（可能很多）您的程式碼無法如預期運作。 AEM是一個功能強大的平台，但強大的功能……帶來極大的複雜性。 以下是疑難排解和追蹤問題時的幾個高層級起點（但遠非可能出錯的完整清單）：

### 驗證程式碼部署

遇到問題時，正確的第一步是確認程式碼已成功部署並安裝到AEM。

1. **Check [!UICONTROL 封裝管理員]** 若要確保已上傳並安裝程式碼套件： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 檢查時間戳記，確認套件最近已安裝。
1. 如果使用如下工具執行增量檔案更新 [!DNL Repo] 或 [!DNL AEM Developer Tools]， **check[!DNL CRXDE Lite]** 檔案已推送至本機AEM執行個體，且檔案內容已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **檢查是否已上傳套件組合** 如果在OSGi套件組合中看到與Java™程式碼相關的問題。 開啟 [!UICONTROL Adobe Experience Manager Web Console]： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 和搜尋您的套件組合。 確保套件組合具有 **[!UICONTROL 作用中]** 狀態。 請參閱以下內容，了解疑難排解中的套件組合的相關詳細資訊。 **[!UICONTROL 已安裝]** 州別。

#### 檢查記錄

AEM是一個聊天平台，會將有用的資訊記錄在 **error.log**. 此 **error.log** 可以找到AEM的安裝位置： &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

追蹤問題的實用技術是在Java™程式碼中新增記錄陳述式：

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

根據預設 **error.log** 已設定為記錄 *[!DNL INFO]* 陳述式。 若要變更記錄層級，您可以前往 [!UICONTROL 記錄檔支援]： [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 您也可能發現 **error.log** 太扯了。 您可以使用 [!UICONTROL 記錄檔支援] 僅針對指定的Java™套件設定log陳述式。 這是專案的最佳實務，以便輕鬆地將自訂程式碼問題與OOTB AEM平台問題分開。

![在AEM中記錄設定](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 套件組合處於已安裝狀態 {#bundle-active}

所有組合（不包括片段）應位於 **[!UICONTROL 作用中]** 州別。 如果您在程式碼套件組合中看到 [!UICONTROL 已安裝] 則有需要解決的問題。 在大多數情況下，這是相依性問題：

![AEM中的套件組合錯誤](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上方熒幕擷圖中， [!DNL WKND Core bundle] 是 [!UICONTROL 已安裝] 州別。 這是因為套件組合預期的是不同版本的 `com.adobe.cq.wcm.core.components.models` 比AEM執行個體上的可用。

一個實用的可用工具是 [!UICONTROL 相依性尋找器]： [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). 新增Java™套件名稱以檢查AEM執行個體上可用的版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

繼續上述範例，我們可以看到AEM執行個體上安裝的版本為 **12.2** 與 **12.6** 套件組合所預期的。 從那裡，您可以向後工作，並檢視 [!DNL Maven] AEM的相依性符合 [!DNL Maven] AEM專案中的相依性。 在中，上述範例 [!DNL Core Components] **v2.2.0** 安裝在AEM執行個體上，但程式碼套件組合是以相依性建置 **v2.2.2**，因此發生相依性問題的原因。

#### 驗證Sling模型註冊 {#osgi-component-sling-models}

AEM元件必須由 [!DNL Sling Model] 封裝任何商業邏輯並確保HTL轉譯指令碼保持乾淨。 如果遇到Sling模型找不到的問題，檢視 [!DNL Sling Models] 從主控台： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 這會告訴您是否已註冊Sling模型，以及它繫結的資源型別（元件路徑）。

![Sling模型狀態](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

顯示註冊 [!DNL Sling Model]， `BylineImpl` 繫結至下列元件資源型別： `wknd/components/content/byline`.

#### CSS或JavaScript問題

針對大多數CSS和JavaScript問題，使用瀏覽器的開發工具是進行疑難排解的最有效方式。 若要在針對AEM作者執行個體進行開發時縮小問題範圍，以「已發佈」形式檢視頁面會很有幫助。

![CSS或JS問題](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

開啟 [!UICONTROL 頁面屬性] 功能表並按一下 [!UICONTROL 檢視已發佈]. 這會開啟頁面，但不使用AEM編輯器，且查詢引數設定為 **wcmmode=disabled**. 這實際上會停用AEM編寫UI，並讓疑難排解/偵錯前端問題變得更容易。

開發前端程式碼時另一個常見問題為過時或載入了過期的CSS/JS。 首先，請確定瀏覽器歷史記錄已清除，並視需要啟動無痕瀏覽器或全新工作階段。

#### 偵錯使用者端程式庫

使用不同的類別和嵌入方法來包含多個使用者端程式庫，進行疑難排解可能會很麻煩。 AEM會公開數個工具來協助解決此問題。 最重要的工具之一是 [!UICONTROL 重建使用者端資料庫] 會強制AEM重新編譯任何LESS檔案並產生CSS。

* [傾印程式庫](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM執行個體中註冊的所有使用者端程式庫。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [測試輸出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 可讓使用者根據類別檢視clientlib include的預期HTML輸出。 &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [程式庫相依性驗證](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 反白標示任何找不到的相依性或內嵌類別。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [重建使用者端資料庫](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 可讓使用者強制AEM重建所有使用者端程式庫或讓使用者端程式庫的快取失效。 使用LESS開發時，此工具相當有效，因為這會強制AEM重新編譯產生的CSS。 一般而言，讓快取失效，然後執行頁面重新整理比重建所有程式庫更有效率。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![偵錯Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您必須持續使用，讓快取失效 [!UICONTROL 重建使用者端資料庫] 工具，或許值得一次重建所有使用者端程式庫。 這可能需要約15分鐘，但通常可消除未來的任何快取問題。
