---
title: 自定义组件
description: 涵蓋顯示編寫內容的自訂署名元件的端對端建立。 包括開發Sling模型來封裝商業邏輯，以填入署名元件和對應的HTL來演算元件。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 434f56e143bc0f969723de48abd26d49a308af9b
workflow-type: tm+mt
source-wordcount: '4061'
ht-degree: 0%

---

# 自定义组件 {#custom-component}

本教學課程涵蓋自訂的端對端建立作業 `Byline` 顯示對話方塊中製作內容的AEM元件，並探索開發Sling模型以封裝填入元件HTL的商業邏輯。

## 前提条件 {#prerequisites}

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並跳過出庫入門專案的步驟。

檢視教學課程建置的基礎行程式碼：

1. 檢視 `tutorial/custom-component-start` 分支來源 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. 使用您的Maven技能將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 或切換至分支以在本機簽出程式碼 `tutorial/custom-component-solution`.

## 目标

1. 瞭解如何建立自訂AEM元件
1. 瞭解如何使用Sling模型封裝商業邏輯
1. 瞭解如何從HTL指令碼使用Sling模型

## 您即將建置的內容 {#what-build}

在WKND教學課程的這個部分中，會建立署名元件，用於顯示有關文章投稿人的撰寫資訊。

![署名元件範例](assets/custom-component/byline-design.png)

*署名元件*

署名元件的實作包含一個收集署名內容的對話方塊，以及一個自訂Sling模型，可擷取詳細資訊，例如：

* 名称
* 图像
* 職業

## 建立署名元件 {#create-byline-component}

首先，建立「署名元件」節點結構並定義對話方塊。 這代表AEM中的元件，並依據元件在JCR中的位置來隱含定義元件的資源型別。

此對話方塊會顯示內容作者可提供的介面。 對於此實作，AEM WCM核心元件的 **影像** 元件是用來處理Byline影像的製作和演算，因此必須將其設定為此元件的 `sling:resourceSuperType`.

### 建立元件定義 {#create-component-definition}

1. 在 **ui.apps** 模組，導覽至 `/apps/wknd/components` 並建立名為的資料夾 `byline`.
1. 內部 `byline` 資料夾，新增名為 `.content.xml`

   ![建立節點的對話方塊](assets/custom-component/byline-node-creation.png)

1. 填入 `.content.xml` 檔案：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML檔案提供元件的定義，包括標題、說明和群組。 此 `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，也就是 [核心影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### 建立HTL指令碼 {#create-the-htl-script}

1. 內部 `byline` 資料夾，新增檔案 `byline.html`，負責元件的HTML表示。 將檔案命名為與資料夾相同的名稱很重要，因為它會成為Sling用來呈現此資源型別的預設指令碼。

1. 將下列程式碼新增至 `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

此 `byline.html` 是 [稍後重新檢視](#byline-htl)，建立Sling模型後。 HTL檔案的目前狀態可讓元件在AEM Sites的頁面編輯器中拖放至頁面時以空白狀態顯示。

### 建立對話方塊定義 {#create-the-dialog-definition}

接下來，使用下列欄位為署名元件定義對話方塊：

* **名稱**：投稿人名稱的文字欄位。
* **影像**：投稿人簡歷的參考。
* **職業**：歸屬於貢獻者的職業清單。 職務應依字母遞增順序排序（a至z）。

1. 內部 `byline` 資料夾，建立名為的資料夾 `_cq_dialog`.
1. 內部 `byline/_cq_dialog`，新增名為的檔案 `.content.xml`. 這是對話方塊的XML定義。 新增下列XML：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
                       <properties
                               jcr:primaryType="nt:unstructured"
                               jcr:title="Properties"
                               sling:resourceType="granite/ui/components/coral/foundation/container"
                               margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                       jcr:primaryType="nt:unstructured"
                                       sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                       margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                               jcr:primaryType="nt:unstructured"
                                               sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   這些對話方塊節點定義使用 [Sling資源合併](https://sling.apache.org/documentation/bundles/resource-merger.html) 控制從繼承哪些對話方塊標籤 `sling:resourceSuperType` 元件，在此案例中為 **核心元件的影像元件**.

   ![署名對話方塊已完成](assets/custom-component/byline-dialog-created.png)

### 建立原則對話方塊 {#create-the-policy-dialog}

遵循與建立對話方塊相同的方法，建立原則對話方塊（以前稱為設計對話方塊）以在從核心元件的影像元件繼承的原則設定中隱藏不需要的欄位。

1. 內部 `byline` 資料夾，建立名為的資料夾 `_cq_design_dialog`.
1. 內部 `byline/_cq_design_dialog`，建立名為的檔案 `.content.xml`. 使用下列XML更新檔案：。 最簡單的做法是開啟 `.content.xml` 並將下方的XML複製/貼上至其中。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   前一個專案的基礎 **原則對話方塊** XML是從 [核心元件影像元件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   就像在對話方塊設定中， [Sling資源合併](https://sling.apache.org/documentation/bundles/resource-merger.html) 用於隱藏原本繼承自下列專案的不相關欄位： `sling:resourceSuperType`，如下列節點定義所示： `sling:hideResource="{Boolean}true"` 屬性。

### 部署程式碼 {#deploy-the-code}

1. 同步化變更 `ui.apps` 使用IDE或使用Maven技能。

   ![匯出至AEM伺服器署名元件](assets/custom-component/export-byline-component-aem.png)

## 將元件新增至頁面 {#add-the-component-to-a-page}

為了簡單起見，並專注於AEM元件的開發，讓我們將Byline元件以目前狀態新增至「文章」頁面，以驗證 `cq:Component` 節點定義正確。 另外還要確認AEM可辨識新元件定義，且元件的對話方塊可用於編寫。

### 將影像新增至AEM Assets

首先，將擷取的頭像範例上傳至AEM Assets，以便用於填入Byline元件中的影像。

1. 導覽至AEM Assets中的LA Skateparks資料夾： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. 上傳頭部快照  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 至資料夾。

   ![大頭照已上傳至AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 編寫元件 {#author-the-component}

接下來，將Byline元件新增至AEM中的頁面。 因為Byline元件已新增至 **WKND Sites專案 — 內容** 元件群組，透過 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定義，則任何使用者均可自動使用 **容器** 其 **原則** 允許 **WKND Sites專案 — 內容** 元件群組。 因此，文章頁面的版面容器中可使用此功能。

1. 導覽至LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 從左側邊欄，拖放 **署名元件** 開啟至 **bottom** 已開啟文章頁面的「版面容器」的頁首。

   ![將署名元件新增至頁面](assets/custom-component/add-to-page.png)

1. 確定左側邊欄已開啟&#x200B;**和可見，以及**&#x200B;已選取「資產尋找器**」。

1. 選取 **署名元件預留位置**，接著會顯示動作列，然後點選 **扳手** 圖示以開啟對話方塊。

1. 開啟對話方塊，且第一個索引標籤（資產）作用中時，請開啟左側邊欄，然後從資產尋找器將影像拖放至「影像」拖放區域。 搜尋「stacey」以尋找WKND ui.content套件中提供的Stacey Roswells生物圖片。

   ![新增影像至對話方塊](assets/custom-component/add-image.png)

1. 新增影像後，按一下 **屬性** 索引標籤以輸入 **名稱** 和 **職業**.

   輸入職務時，請在 **反向字母順序** order，以便驗證Sling模型中實作的字母順序商業邏輯。

   點選 **完成** 按鈕來儲存變更。

   ![填入署名元件的屬性](assets/custom-component/add-properties.png)

   AEM作者可透過對話方塊設定及編寫元件。 此時，在開發Byline元件時，會包含用於收集資料的對話方塊，但尚未新增轉譯所編寫內容的邏輯。 因此，只會顯示預留位置。

1. 儲存對話方塊後，導覽至 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 和檢閱元件內容如何儲存在AEM頁面下方的署名元件內容節點上。

   尋找「洛杉磯滑板公園」頁面下方的「署名」元件內容節點，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   注意屬性名稱 `name`， `occupations`、和 `fileReference` 儲存在 **署名節點**.

   此外，請留意 `sling:resourceType` 節點的ID設為 `wknd/components/content/byline` 這會將此內容節點繫結至Byline元件實作。

   ![CRXDE中的署名屬性](assets/custom-component/byline-properties-crxde.png)

## 建立署名Sling模型 {#create-sling-model}

接下來，讓我們建立Sling模型以作為資料模型，並存放Byline元件的商業邏輯。

Sling模型是註解導向的Java™ POJO (Plain Old Java™ Objects)，有助於將資料從JCR對應到Java™變數，並在AEM環境中開發時提供效率。

### 檢閱Maven相依性 {#maven-dependency}

署名Sling模型需依賴AEM提供的數個Java™ API。 這些API可透過 `dependencies` 列於 `core` 模組的POM檔案。 本教學課程使用的專案已針對AEMas a Cloud Service建置。 但此版本有其獨特之處，因為可回溯相容於AEM 6.5/6.4。因此，其中同時包含Cloud Service和AEM 6.x的相依性。

1. 開啟 `pom.xml` 下的檔案 `<src>/aem-guides-wknd/core/pom.xml`.
1. 尋找相依性 `aem-sdk-api` - **僅限AEMas a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   此 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) 包含AEM公開的所有公用Java™ API。 此 `aem-sdk-api` 建立此專案時預設會使用。 此版本會保留在專案根目錄的Parent reactor pom中 `aem-guides-wknd/pom.xml`.

1. 尋找的相依性 `uber-jar` - **僅限AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   此 `uber-jar` 僅包含在 `classic` 叫用設定檔，即 `mvn clean install -PautoInstallSinglePackage -Pclassic`. 同樣地，此為專案所特有。 在真實世界專案中，從AEM專案原型產生 `uber-jar` 若指定的AEM版本為6.5或6.4，則為預設值。

   此 [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) 包含AEM 6.x公開的所有公用Java™ API。此版本會保留在專案根目錄的Parent reactor pom中 `aem-guides-wknd/pom.xml`.

1. 尋找相依性 `core.wcm.components.core`：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   這是由AEM核心元件公開的完整公用Java™ API。 AEM核心元件是在AEM外部維護的專案，因此有單獨的發行週期。 因此，相依性需要單獨納入，且 **not** 包含在 `uber-jar` 或 `aem-sdk-api`.

   和uber-jar一樣，此相依性的版本會保留在來自的父Reactor pom檔案中 `aem-guides-wknd/pom.xml`.

   在本教學課程的稍後部分，將使用「核心元件影像」類別來顯示Byline元件中的影像。 為了建置和編譯Sling模型，必須有核心元件相依性。

### 署名介面 {#byline-interface}

為署名建立公用Java™介面。 此 `Byline.java` 定義驅動 `byline.html` HTL指令碼。

1. 內部， `core` 內的模組 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾建立名為的檔案 `Byline.java`

   ![建立署名介面](assets/custom-component/create-byline-interface.png)

1. 更新 `Byline.java` 方法而可行：

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   前兩種方法會公開 **名稱** 和 **職業** （署名元件）。

   此 `isEmpty()` 方法可用來決定元件是否有任何要呈現的內容，或元件是否等待設定。

   請注意，影像沒有方法； [稍後會檢閱此內容](#tackling-the-image-problem).

1. 包含公用Java™類別的Java™套件（在此例中為Sling模型）必須使用套件的版本設定  `package-info.java` 檔案。

   由於WKND來源的Java™套件 `com.adobe.aem.guides.wknd.core.models` 宣告版本 `1.0.0`，且正在新增不斷行公用介面和方法，版本必須增加至 `1.1.0`. 開啟檔案於 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 和更新 `@Version("1.0.0")` 至 `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

每當對此封裝中的檔案進行變更時， [套件版本必須在語義上調整](https://semver.org/). 如果沒有，Maven專案的 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) 會偵測到無效的封裝版本，並中斷內建。 幸運的是，失敗時，Maven外掛程式會報告無效的Java™套件版本及其應該使用的版本。 更新 `@Version("...")` 違反的Java™套件中的宣告 `package-info.java` 至外掛程式建議修正的版本。

### 署名實施 {#byline-implementation}

此 `BylineImpl.java` 是實作的Sling模型的實作 `Byline.java` 介面之前已定義。 的完整程式碼 `BylineImpl.java` 可在此章節底部找到。

1. 建立名為的資料夾 `impl` 下 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 在 `impl` 資料夾，建立檔案 `BylineImpl.java`.

   ![署名實作檔案](assets/custom-component/byline-impl-file.png)

1. 打开 `BylineImpl.java`. 指定它實作 `Byline` 介面。 使用IDE的自動完成功能或手動更新檔案，以包含實作 `Byline` 介面：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. 透過更新新增Sling模型註釋 `BylineImpl.java` 類別層級的註解。 此 `@Model(..)`註解會將類別轉換為Sling模型。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   讓我們檢閱此註解及其引數：

   * 此 `@Model` annotation在部署至AEM時將BylineImpl註冊為Sling模型。
   * 此 `adaptables` 引數會指定此模型可依請求調整。
   * 此 `adapters` 引數允許實作類別在Byline介面下註冊。 這可讓HTL指令碼透過介面呼叫Sling模型（而不是直接實作）。 [如需有關介面卡的更多詳細資料，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * 此 `resourceType` 指向Byline元件資源型別（先前建立），並在有多個實作時協助解決正確的模型。 [有關將模型類別與資源型別相關聯的更多詳細資訊，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### 實施Sling模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

第一個實作的方法是 `getName()`，則只會傳回儲存在屬性下署名JCR內容節點中的值 `name`.

對此， `@ValueMapValue` Sling模型註解是用來使用請求資源的ValueMap，將值插入Java™欄位。


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

由於JCR屬性會將名稱共用為Java™欄位（兩者都是「name」）， `@ValueMapValue` 會自動解析此關聯，並將屬性的值插入Java™欄位中。

#### getOccupations() {#implementing-get-occupations}

下一個實作方法是 `getOccupations()`. 此方法會載入儲存在JCR屬性中的職業 `occupations` 並傳回這些專案的已排序（按字母順序）集合。

使用在中探索的相同技術 `getName()` 屬性值可插入Sling模型的欄位中。

一旦JCR屬性值透過插入的Java™欄位在Sling模型中可用 `occupations`，排序商業邏輯可套用在 `getOccupations()` 方法。


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

最後一個公用方法是 `isEmpty()` 這會決定元件何時應將其自身視為「已創作完畢」而可呈現。

對於此元件，業務需求是所有三個欄位， `name, image and occupations` 必須填寫 *早於* 元件可以轉譯。


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### 處理「影像問題」 {#tackling-the-image-problem}

檢查名稱和佔用條件並不重要，Apache Commons Lang3提供了便利性 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 類別。 然而，目前還不清楚 **影像是否存在** 可驗證，因為核心元件影像元件是用來對影像進行表面。

有兩種方法可以解決這個問題：

檢查 `fileReference` JCR屬性解析為資產。 *或* 將此資源轉換為核心元件影像Sling模型，並確保 `getSrc()` 方法不是空的。

讓我們使用 **秒** 方法。 第一種方式可能就足夠了，但在本教學課程中，後者是用來讓我們探索Sling模型的其他功能。

1. 建立取得影像的私人方法。 此方法會保留為私用，因為不需要在HTL本身中公開影像物件，而且僅用於驅動 `isEmpty().`

   新增以下私人方法 `getImage()`：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，還有兩種方法可取得 **影像Sling模型**：

   第一個使用 `@Self` 註解，自動調整目前請求以符合核心元件的 `Image.class`

   第二個使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服務，這項方便易用的服務，可協助我們在Java™程式碼中建立其他型別的Sling模型。

   讓我們使用第二個方法。

   >[!NOTE]
   >
   >在真實世界的實作中，方法「一」，使用 `@Self` 較偏好使用，因為這是更簡單、更優雅的解決方案。 在本教學課程中，將會使用第二個方法，因為它需要探索更多實用的Sling模型面向（較複雜的元件）！

   由於Sling模型是Java™ POJO的，而不是OSGi服務，因此通常的OSGi插入註解 `@Reference` **無法** 使用，而Sling模型會提供 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 提供類似功能的註解。

1. 更新 `BylineImpl.java` 以包含 `OSGiService` 註解以插入 `ModelFactory`：

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   使用 `ModelFactory` 在可用時，可以使用以下專案建立核心元件影像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   然而，此方法需要請求和資源，而Sling模型中尚未提供這兩個資源。 若要取得這些註釋，請使用更多Sling模型註釋！

   若要取得目前請求，請 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 註解可用來插入 `adaptable` (定義於 `@Model(..)` 作為 `SlingHttpServletRequest.class`，放入Java™類別欄位中。

1. 新增 **@Self** 附註以取得 **SlingHttpServletRequest要求**：

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   記住，使用 `@Self Image image` 以上可選擇插入核心元件影像Sling模型 —  `@Self` 註解會嘗試插入可調整的物件（在此例中為SlingHttpServletRequest），並調整成註解欄位型別。 由於核心元件影像Sling模型可從SlingHttpServletRequest物件改寫，因此這原本可以運作，且程式碼比探索性更少 `modelFactory` 方法。

   現在會插入透過ModelFactory API例項化影像模型所需的變數。 讓我們使用Sling模型的 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 註解，以便在Sling模型例項化後取得此物件。

   `@PostConstruct` 非常有用，而且作用與建構函式類似，不過，它會在類別例項化並插入所有已註解Java™欄位後叫用。 而其他Sling模型註解會註解Java™類別欄位（變數）， `@PostConstruct` 註解void，zero引數方法，通常名為 `init()` （但任何名稱皆可命名）。

1. 新增 **@PostConstruct** 方法：

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   請記住，Sling模型是 **NOT** OSGi服務，所以維護類別狀態是安全的。 經常 `@PostConstruct` 衍生及設定Sling模型類別狀態以供日後使用，類似於普通建構函式的用途。

   如果 `@PostConstruct` 方法擲回例外狀況、Sling模型未例項化且為null。

1. **getImage()** 現在可以更新為只傳回影像物件。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 讓我們回到 `isEmpty()` 並完成實作：

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   記下多個來電至 `getImage()` 沒有問題，因為傳回初始化的 `image` 類別變數且不會叫用 `modelFactory.getModelFromWrappedRequest(...)` 這並非過於昂貴，但值得避免不必要的電話。

1. 最終版 `BylineImpl.java` 應如下所示：


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## 署名HTL {#byline-htl}

在 `ui.apps` 模組，開啟 `/apps/wknd/components/byline/byline.html` 之前的AEM元件設定中建立的元件。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

讓我們檢閱此HTL指令碼目前為止的作用：

* 此 `placeholderTemplate` 指向核心元件的預留位置，此預留位置會在元件未完全設定時顯示。 這會在AEM Sites頁面編輯器中呈現為具有元件標題的方塊，如上述的 `cq:Component`的  `jcr:title` 屬性。

* 此 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 載入 `placeholderTemplate` 以上定義並傳入布林值(目前硬式編碼為 `false`)放入預留位置範本中。 時間 `isEmpty` 為true，預留位置範本會呈現灰色方塊，否則不會呈現任何內容。

### 更新署名HTL

1. 更新 **byline.html** 具有下列骨架HTML結構：

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   請注意，CSS類別會遵循 [BEM命名慣例](https://getbem.com/naming/). 雖然不強制使用BEM慣例，但建議使用BEM，因為它用於核心元件CSS類別，且通常會產生乾淨且可讀取的CSS規則。

### 在HTL中具現化Sling模型物件 {#instantiating-sling-model-objects-in-htl}

此 [Use區塊陳述式](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 用來在HTL指令碼中具現化Sling模型物件，並將其指派給HTL變數。

此 `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 會使用BylineImpl實作的Byline介面(com.adobe.aem.guides.wknd.models.Byline)，並將目前的SlingHttpServletRequest調整至該介面，而結果會儲存在HTL變數名稱byline ( `data-sly-use.<variable-name>`)。

1. 更新外部 `div` 以參照 **署名** Sling模型公用介面：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 存取Sling模型方法 {#accessing-sling-model-methods}

HTL會從JSTL借入，並使用與Java™ getter方法名稱相同的縮短格式。

例如，叫用Byline Sling模型的 `getName()` 方法可縮短為 `byline.name`，同樣地，而非 `byline.isEmpty`，這可以簡稱為 `byline.empty`. 使用完整方法名稱， `byline.getName` 或 `byline.isEmpty`也一樣。 請注意 `()` 絕不會用來叫用HTL中的方法（類似JSTL）。

需要引數的Java™方法 **無法** 用於HTL。 這是為了讓HTL的邏輯維持簡單。

1. 可以透過叫用 `getName()` 方法（使用Byline Sling模型或HTL）： `${byline.name}`.

   更新 `h2` 標籤：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL運算式選項 {#using-htl-expression-options}

[HTL運算式選項](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 在HTL中做為內容的修飾元，範圍從日期格式到i18n翻譯。 運算式也可用來聯結值清單或陣列，以逗號分隔格式顯示位置時需要用到這些清單。

運算式是透過 `@` 運算式中的運運算元。

1. 若要以「， 」加入職業清單，請使用下列程式碼：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有條件地顯示預留位置 {#conditionally-displaying-the-placeholder}

AEM元件的大多數HTL指令碼都會使用 **預留位置範例** 為作者提供視覺提示 **指出元件的編寫不正確，且不會顯示在AEM Publish上**. 推動此決定的慣例是在元件的後援Sling模型上實作方法，在此案例中為： `Byline.isEmpty()`.

此 `isEmpty()` 方法會在Byline Sling模型上叫用，而結果（或更確切地說，是負值）是透過 `!` 運運算元)儲存至名為的HTL變數 `hasContent`：

1. 更新外部 `div` 儲存名為的HTL變數 `hasContent`：

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   請注意的使用 `data-sly-test`，HTL `test` 區塊是索引鍵，它會設定HTL變數，並轉譯/不轉譯其所使用的HTML元素。 這是以HTL運算式評估的結果為基礎。 如果為「true」，HTML元素會呈現，否則不會呈現。

   此HTL變數 `hasContent` 現在可重複使用來有條件地顯示/隱藏預留位置。

1. 將條件式呼叫更新為 `placeholderTemplate` 檔案底部包含以下專案：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心元件顯示影像 {#using-the-core-components-image}

適用於的HTL指令碼 `byline.html` 即將完成，僅遺失影像。

作為 `sling:resourceSuperType` 指向核心元件的影像元件以製作影像，核心元件的影像元件可用於轉譯影像。

為此，讓我們包含目前的署名資源，但使用資源型別強制使用核心元件影像元件的資源型別 `core/wcm/components/image/v2/image`. 這是元件重複使用的強大模式。 為此，HTL的 `data-sly-resource` 區塊已使用。

1. 取代 `div` 具有類別 `cmp-byline__image` ，其功能如下：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此 `data-sly-resource`，包括透過相對路徑的目前資源 `'.'`，和會強制納入目前資源（或署名內容資源），資源型別為 `core/wcm/components/image/v2/image`.

   核心元件資源型別會直接使用，而非透過Proxy使用，因為這是指令碼內使用，且永遠不會保留至內容。

2. 已完成 `byline.html` 下：

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. 將程式碼基底部署至本機AEM執行個體。 由於對下列專案進行了變更： `core` 和 `ui.apps` 兩個模組都需要部署。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   若要部署至AEM 6.5/6.4，請叫用 `classic` 設定檔：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您也可以使用Maven設定檔從根建立整個專案 `autoInstallSinglePackage` 但這可能會覆寫頁面上的內容變更。 這是因為 `ui.content/src/main/content/META-INF/vault/filter.xml` 已針對教學課程入門程式碼進行修改，以完全覆寫現有的AEM內容。 在真實世界中，這並不是問題。

### 檢閱未設定樣式的署名元件 {#reviewing-the-unstyled-byline-component}

1. 部署更新後，導覽至 [LA滑板公園終極指南 ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 頁面上，或在章節中先前新增署名元件的位置。

1. 此 **影像**， **名稱**、和 **職業** 現在會出現，並顯示一個未設定樣式，但運作中的「署名」元件。

   ![未設定樣式的署名元件](assets/custom-component/unstyled.png)

### 檢閱Sling模型註冊 {#reviewing-the-sling-model-registration}

此 [AEM Web主控台的Sling模型狀態檢視](http://localhost:4502/system/console/status-slingmodels) 顯示AEM中所有已註冊的Sling模型。 您可以檢閱此清單，以驗證Byline Sling模型是否已安裝及識別。

如果 **署名實作** 不會顯示在此清單中，這可能是因為Sling模型的註解有問題，或是模型未新增到正確的套件(`com.adobe.aem.guides.wknd.core.models`)時，不會隱藏任何專案。

![署名Sling模型已註冊](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名樣式 {#byline-styles}

若要將「署名」元件與提供的創意設計對齊，請設定其樣式。 這是透過使用SCSS檔案並更新 **ui.frontend** 模組。

### 新增預設樣式

為Byline元件新增預設樣式。

1. 返回IDE和 **ui.frontend** 專案在 `/src/main/webpack/components`：
1. 建立名為的檔案 `_byline.scss`.

   ![署名專案總管](assets/custom-component/byline-style-project-explorer.png)

1. 將Byline實作CSS （寫入為SCSS）新增至 `_byline.scss`：

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. 開啟終端機並導覽至 `ui.frontend` 模組。
1. 開始 `watch` 使用下列npm命令處理：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回瀏覽器並導覽至 [LA SkateParks文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 您應該會看到元件的更新樣式。

   ![完成的署名元件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除瀏覽器快取以確保未提供過時的CSS，然後重新整理頁面以使用署名元件來取得完整樣式。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager從頭開始建立自訂元件！

### 后续步骤 {#next-steps}

探索如何為Byline Java™程式碼撰寫JUnit測試，以繼續瞭解AEM元件開發，確保所有專案皆已正確開發，且實作的商業邏輯正確且完整。

* [寫入單元測試或AEM元件](unit-testing.md)

檢視完成的程式碼： [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上檢閱並部署程式碼 `tutorial/custom-component-solution`.

1. 原地複製 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 檢視 `tutorial/custom-component-solution` 分支
