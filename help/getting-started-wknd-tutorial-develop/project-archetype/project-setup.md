---
title: 開始使用AEM Sites — 專案設定
description: 建立Maven Multi Module專案以管理Experience Manager網站的程式碼和設定。
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '1839'
ht-degree: 4%

---

# 项目设置 {#project-setup}

本教學課程說明如何建立Maven Multi Module專案，以管理Adobe Experience Manager網站的程式碼和設定。

## 前提条件 {#prerequisites}

檢閱設定「 」所需的工具和指示 [本機開發環境](./overview.md#local-dev-environment). 確保您有本機可用的全新Adobe Experience Manager執行個體，且尚未安裝其他範例/示範套件（必要服務套件除外）。

## 目标 {#objective}

1. 瞭解如何使用Maven原型產生新的AEM專案。
1. 瞭解AEM專案原型產生的不同模組，以及它們如何共同運作。
1. 瞭解AEM專案中如何包含AEM核心元件。

## 您即將建置的內容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您會使用產生新的Adobe Experience Manager專案 [AEM專案原型](https://github.com/adobe/aem-project-archetype). 您的AEM專案包含用於Sites實作的完整程式碼、內容和設定。 本章產生的專案可作為WKND網站實作的基礎，並在未來的章節中建置。

**什麼是Maven專案？** - [Apache Maven](https://maven.apache.org/) 是用於建立專案的軟體管理工具。 *所有Adobe Experience Manager* 實作使用Maven專案在AEM上建置、管理和部署自訂程式碼。

**什麼是Maven原型？** - A [Maven原型](https://maven.apache.org/archetype/index.html) 是用於產生新專案的範本或模式。 AEM專案原型有助於產生具有自訂名稱空間的新專案，並包括遵循最佳實務的專案結構，大幅加快專案開發。

## 建立專案 {#create}

建立適用於AEM的Maven多模組專案有幾個選項。 本教學課程使用 [Maven AEM專案原型 **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager也 [提供使用者介面精靈](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html) 以開始建立AEM應用程式專案。 Cloud Manager UI產生的基礎專案會產生與直接使用原型相同的結構。

>[!NOTE]
>
>本教學課程使用版本 **35** 原型的。 最佳實務就是使用 **最新** 用於產生新專案的原型版本。

下一系列步驟將使用基於UNIX®的命令列終端機進行，但如果使用Windows終端機，則應該類似。

1. 開啟命令列終端機。 確認已安裝Maven：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 導覽至您要產生AEM專案的目錄。 這可以是您想要維護專案原始程式碼的任何目錄。 例如，名為的目錄 `code` 在使用者主目錄下方：

   ```shell
   $ cd ~/code
   ```

1. 將下列內容貼到命令列以 [以批次模式產生專案](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)：

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 目標AEM 6.5.14+取代 `aemVersion="cloud"` 替換為 `aemVersion="6.5.14"`.
   >
   > 此外，請一律使用最新的 `archetypeVersion` 藉由參考 [AEM專案原型>使用狀況](https://github.com/adobe/aem-project-archetype#usage)

   設定專案的可用屬性完整清單 [可在此處找到](https://github.com/adobe/aem-project-archetype#available-properties).

1. 以下資料夾和檔案結構是由本機檔案系統上的Maven原型產生的：

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 部署和建置專案 {#build}

建置專案程式碼並將其部署到AEM的本機執行個體。

1. 確定您有AEM的製作執行個體在連線埠上本機執行 **4502**.
1. 從命令列，瀏覽至 `aem-guides-wknd` 專案目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 執行以下命令，建置整個專案並將其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   建置大約需要一分鐘的時間，並且應該以下列訊息結束：

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Maven設定檔 `autoInstallSinglePackage` 編譯專案的個別模組，並將單一套件部署至AEM執行個體。 依預設，此套件會部署至在本機於連線埠上執行的AEM執行個體 **4502** 且具備以下憑證： `admin:admin`.

1. 導覽至本機AEM執行個體上的封裝管理員： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您應該會看到以下專案的套件： `aem-guides-wknd.ui.apps`， `aem-guides-wknd.ui.config`， `aem-guides-wknd.ui.content`、和 `aem-guides-wknd.all`.

1. 導覽至Sites主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND網站是其中一個網站。 其中包含具有美國和語言主版階層的網站結構。 此網站階層是根據 `language_country` 和 `isSingleCountryWebsite` 使用原型產生專案時。

1. 開啟 **US** `>` **英文** 頁面，方法是選取頁面並按一下 **編輯** 功能表列中的按鈕：

   ![網站主控台](assets/project-setup/aem-sites-console.png)

1. 已建立入門內容，且有數個元件可新增至頁面。 嘗試使用這些元件，瞭解其功能。 您將在下一章中學習元件的基本知識。

   ![首頁入門內容](assets/project-setup/start-home-page.png)

   *原型產生的範例內容*

## Inspect專案 {#project-structure}

產生的AEM專案由個別Maven模組組成，每個模組都有不同的角色。 本教學課程和大部分的開發工作都專注於這些模組：

* [核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java程式碼，主要是後端開發人員。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、TypeScript的原始程式碼，主要用於前端開發人員。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含元件和對話方塊定義，將編譯的CSS和JavaScript內嵌為使用者端程式庫。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含結構化內容和設定，例如可編輯的範本、中繼資料結構(/content、/conf)。

* **全部**  — 這是一個空的Maven模組，它將上述模組合併成可以部署到AEM環境的單一套件。

![Maven專案圖表](assets/project-setup/project-pom-structure.png)

請參閱 [AEM專案原型檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 以深入瞭解 **全部** Maven模組。

### 包含核心元件 {#core-components}

[AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 是一組適用於AEM的標準化網頁內容管理(WCM)元件。 這些元件提供一組基準功能，並針對個別專案進行樣式、自訂和延伸。

AEMas a Cloud Service環境包含最新版本的 [AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans). 因此，針對AEMas a Cloud Service產生的專案會 **not** 納入AEM核心元件的內嵌。

對於AEM 6.5/6.4產生的專案，原型會自動嵌入 [AEM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 在專案中。 AEM 6.5/6.4的最佳實務是內嵌AEM核心元件，以確保在您的專案中部署最新版本。 有關核心元件運作方式的詳細資訊 [您可以在此處找到專案中包含的](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 原始檔控制管理 {#source-control}

最好是使用某種形式的原始檔控制來管理應用程式中的程式碼。 本教學課程使用Git和GitHub。 Maven和/或所選IDE會產生數個檔案，SCM應忽略這些檔案。

每當您建置和安裝程式碼套件時，Maven都會建立目標資料夾。 目標資料夾和內容應從SCM排除。

在底下 `ui.apps` 模組觀察到許多 `.content.xml` 檔案隨即建立。 這些XML檔案會對應安裝在JCR中的節點型別和內容屬性。 這些檔案非常重要，而且 **無法** 將被忽略。

AEM專案原型會產生範例 `.gitignore` 可作為起始點的檔案，可安全地忽略這些檔案。 檔案產生於 `<src>/aem-guides-wknd/.gitignore`.

## 恭喜！ {#congratulations}

恭喜，您已建立您的第一個AEM專案！

### 后续步骤 {#next-steps}

透過簡單的步驟瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術 `HelloWorld` 範例： [元件基本知識](component-basics.md) 教學課程。

## 進階Maven命令（額外功能） {#advanced-maven-commands}

在開發期間，您可能只使用其中一個模組，並且想要避免建置整個專案以節省時間。 您也可以直接部署至AEM Publish執行個體，或部署至未在連線埠4502上執行的AEM執行個體。

接下來，讓我們檢閱一些其他Maven設定檔和命令，您可以在開發期間使用這些設定檔和命令，以獲得更大的彈性。

### 核心模組 {#core-module}

此 **[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 模組包含與專案相關聯的所有Java™程式碼。 的建置 **核心** 模組會將OSGi套件組合部署至AEM。 若要僅建置此模組：

1. 導覽至 `core` 資料夾（下方） `aem-guides-wknd`)：

   ```shell
   $ cd core/
   ```

1. 執行以下命令：

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. 導覽至 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). 這是OSGi Web主控台，包含有關安裝在AEM執行個體上的所有套件組合的資訊。

1. 切換 **Id** 排序欄，您應該會看到已安裝且作用中的WKND組合。

   ![核心套裝](assets/project-setup/wknd-osgi-console.png)

1. 您可以在中看到jar的「實體」位置 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)：

   ![Jar的CRXDE位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模組 {#apps-content-module}

此 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模組包含底下網站所需的所有轉譯程式碼 `/apps`. 這包括以名為的AEM格式儲存的CSS/JS [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 這也包括 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 用於呈現動態HTML的指令碼。 您可以將 **ui.apps** 模組對應至JCR中的結構，但格式可以儲存在檔案系統上，並認可至原始檔控制。 此 **ui.apps** 模組僅包含程式碼。

若要僅建置此模組：

1. 從命令列。 導覽至 `ui.apps` 資料夾（下方） `aem-guides-wknd`)：

   ```shell
   $ cd ../ui.apps
   ```

1. 執行以下命令：

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 導覽至 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您應該會看到 `ui.apps` 封裝作為第一個安裝的封裝，而且其時間戳記應比任何其他封裝都新。

   ![已安裝Ui.apps套件](assets/project-setup/ui-apps-package.png)

1. 返回命令列並執行以下命令(在 `ui.apps` 資料夾)：

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   設定檔 `autoInstallPackagePublish` 用於將套件部署到在連線埠上執行的發佈環境 **4503**. 如果找不到在http://localhost:4503上執行的AEM執行個體，則會發生上述錯誤。

1. 最後，執行下列命令以部署 `ui.apps` 連線埠上的套件 **4504**：

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   如果連線埠上沒有執行AEM執行個體，同樣會發生建置失敗 **4504** 可用。 引數 `aem.port` 在POM檔案中定義 `aem-guides-wknd/pom.xml`.

此 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 模組的結構與相同 **ui.apps** 模組。 唯一的區別是 **ui.content** 模組包含所謂的 **可變** 內容。 **可變** 內容基本上是指非程式碼設定，例如儲存在原始檔控制中的範本、原則或資料夾結構 **但是** 可直接在AEM執行個體上修改。 如需詳細資訊，請參閱頁面和範本一章。

用來建置 **ui.apps** 模組可用於建置 **ui.content** 模組。 歡迎您從 **ui.content** 資料夾。

## 疑难解答

如果使用AEM專案原型產生專案時發生問題，請參閱 [已知問題](https://github.com/adobe/aem-project-archetype#known-issues) 和開啟的清單 [問題](https://github.com/adobe/aem-project-archetype/issues).

## 再次恭喜！ {#congratulations-bonus}

恭喜您瀏覽獎金資料。

### 后续步骤 {#next-steps-bonus}

透過簡單的步驟瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術 `HelloWorld` 範例： [元件基本知識](component-basics.md) 教學課程。
