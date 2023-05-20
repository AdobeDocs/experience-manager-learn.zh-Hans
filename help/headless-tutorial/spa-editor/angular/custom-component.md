---
title: 建立自訂元件 | AEM SPA編輯器和Angular快速入門
description: 瞭解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# 建立自訂元件 {#custom-component}

瞭解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。

## 目标

1. 瞭解Sling模型在操控AEM提供的JSON模型API方面的作用。
2. 瞭解如何建立AEM元件對話方塊。
3. 瞭解如何建立 **自訂** 與AEM編輯器架構相容的SPA元件。

## 您將建置的內容

前幾章的重點是開發SPA元件，並將它們對應至 *現有* AEM核心元件。 本章著重於如何建立和擴充 *新* AEM元件和操作AEM提供的JSON模型。

簡單 `Custom Component` 說明建立全新AEM元件所需的步驟。

![以全部大寫字顯示的訊息](assets/custom-component/message-displayed.png)

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. 使用Maven將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   若使用 [AEM 6.x](overview.md#compatibility) 新增 `classic` 設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝完成的傳統套件 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest). 提供的影像 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 在WKND SPA上重複使用。 套件可使用以下方式安裝： [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp).

   ![封裝管理員安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或切換至分支以在本機簽出程式碼 `Angular/custom-component-solution`.

## 定義AEM元件

AEM元件定義為節點和屬性。 在專案中，這些節點和屬性在 `ui.apps` 模組。 接下來，在中建立AEM元件 `ui.apps` 模組。

>[!NOTE]
>
> 上的快速重新整理 [AEM元件的基本知識可能會有所幫助](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 開啟 `ui.apps` 資料夾。
2. 導覽至 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 並建立名為的資料夾 `custom-component`.
3. 建立名為的檔案 `.content.xml` 在 `custom-component` 資料夾。 填入 `custom-component/.content.xml` ，其功能如下：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![建立自訂元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — 識別此節點是AEM元件。

   `jcr:title` 是顯示給內容作者的值，而且 `componentGroup` 決定編寫UI中的元件分組。

4. 在 `custom-component` 資料夾，建立另一個名為的資料夾 `_cq_dialog`.
5. 在 `_cq_dialog` 資料夾建立名為的檔案 `.content.xml` 並填入下列內容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![自訂元件定義](assets/custom-component/dialog-custom-component-defintion.png)

   上述XML檔案會為以下專案產生一個簡單的對話方塊： `Custom Component`. 檔案的關鍵部分是內部 `<message>` 節點。 此對話方塊包含一個簡單的 `textfield` 已命名 `Message` 並將textifeld的值保留至名為的屬性 `message`.

   接著會建立Sling模型，以公開 `message` 屬性透過JSON模型傳遞。

   >[!NOTE]
   >
   > 您可以檢視更多專案 [透過檢視核心元件定義建立對話方塊的範例](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 您也可以檢視其他表單欄位，例如 `select`， `textarea`， `pathfield`，可在下方取得 `/libs/granite/ui/components/coral/foundation/form` 在 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   使用傳統AEM元件、一個 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 通常需要指令碼。 由於SPA會轉譯元件，因此不需要HTL指令碼。

## 建立Sling模型

Sling模型是註解導向的Java™「POJO」(Plain Old Java™物件)，可方便將資料從JCR對應至Java™變數。 [Sling模型](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 通常會用來封裝AEM元件的複雜伺服器端商業邏輯。

在SPA編輯器的內容中，Sling模型會使用透過JSON模型透過功能公開元件的內容。 [Sling模型匯出工具](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=zh-Hans).

1. 在您選擇的IDE中，開啟 `core` 模組。 `CustomComponent.java` 和 `CustomComponentImpl.java` 已建立並作為章節起始程式碼的一部分進行清除。

   >[!NOTE]
   >
   > 若使用Visual Studio Code IDE，則進行安裝可能會有幫助 [Java™擴充功能](https://code.visualstudio.com/docs/java/extensions).

2. 開啟Java™介面 `CustomComponent.java` 於 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`：

   ![CustomComponent.java介面](assets/custom-component/custom-component-interface.png)

   這是由Sling模型實作的Java™介面。

3. 更新 `CustomComponent.java` 以擴充 `ComponentExporter` 介面：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   實作 `ComponentExporter` 介面是JSON模型API自動擷取Sling模型的需求。

   此 `CustomComponent` 介麵包含單一getter方法 `getMessage()`. 這是透過JSON模型公開作者對話方塊值的方法。 僅限具有空白引數的getter方法 `()` 都會匯出為JSON模型。

4. 開啟 `CustomComponentImpl.java` 於 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   此為的實作 `CustomComponent` 介面。 此 `@Model` annotation會將Java™類別識別為Sling模型。 此 `@Exporter` 註解可讓您透過Sling模型匯出程式序列化和匯出Java™類別。

5. 更新靜態變數 `RESOURCE_TYPE` 指向AEM元件 `wknd-spa-angular/components/custom-component` 建立於上一個練習中。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   元件的資源型別會將Sling模型繫結至AEM元件，最終對應至Angular元件。

6. 新增 `getExportedType()` 方法至 `CustomComponentImpl` 類別以傳回元件資源型別：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   實作 `ComponentExporter` 介面並公開允許對應至Angular元件的資源型別。

7. 更新 `getMessage()` 傳回值的方法 `message` 由作者對話方塊保留的屬性。 使用 `@ValueMap` annotation是對應JCR值 `message` 至Java™變數：

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   新增一些額外的「商業邏輯」，以大寫形式傳回訊息的值。 這可讓我們檢視製作對話方塊儲存的原始值和Sling模型公開的值之間的差異。

   >[!NOTE]
   您可以檢視 [已在此完成CustomComponentImpl.java](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## 更新Angular元件

已建立自訂元件的Angular代碼。 接下來，進行一些更新，將Angular元件對應至AEM元件。

1. 在 `ui.frontend` 模組開啟檔案 `ui.frontend/src/app/components/custom/custom.component.ts`
2. 觀察 `@Input() message: string;` 行。 轉換後的大寫值應對應至此變數。
3. 匯入 `MapTo` AEM SPA編輯器JS SDK中的物件，並使用它來對應到AEM元件：

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開啟 `cutom.component.html` 並觀察其值 `{{message}}` 會顯示在側 `<h2>` 標籤之間。
5. 開啟 `custom.component.css` 並新增下列規則：

   ```css
   :host-context {
       display: block;
   }
   ```

   若要在元件為空時正確顯示AEM編輯器預留位置，請 `:host-context` 或其他 `<div>` 需要設定為 `display: block;`.

6. 使用您的Maven技能，從專案目錄的根將更新部署到本機AEM環境：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新範本原則

接下來，導覽至AEM以驗證更新，並允許 `Custom Component` 新增至SPA。

1. 透過瀏覽至以下位置，驗證新Sling模型的註冊： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您應該會看到上面兩行，指出 `CustomComponentImpl` 已與 `wknd-spa-angular/components/custom-component` 元件並透過Sling模型匯出工具註冊。

2. 導覽至SPA頁面範本，網址為 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 更新配置容器的原則以新增 `Custom Component` 作為允許的元件：

   ![更新配置容器原則](assets/custom-component/custom-component-allowed.png)

   儲存對原則的變更，並觀察 `Custom Component` 作為允許的元件：

   ![自訂元件作為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 編寫自訂元件

接下來，編寫 `Custom Component` 使用AEM SPA編輯器。

1. 導覽至 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. 在 `Edit` 模式，新增 `Custom Component` 至 `Layout Container`：

   ![插入新组件](assets/custom-component/insert-custom-component.png)

3. 開啟元件的對話方塊，然後輸入包含一些小寫字母的訊息。

   ![設定自訂元件](assets/custom-component/enter-dialog-message.png)

   這是根據章節中先前的XML檔案建立的對話方塊。

4. 保存更改。請注意，顯示的訊息會全部大寫。

   ![以全部大寫字顯示的訊息](assets/custom-component/message-displayed.png)

5. 導覽至「 」以檢視JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 搜索 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   請注意，根據Sling模型新增的邏輯，JSON值會設定為所有大寫字母。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何建立自訂AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或切換至分支以在本機簽出程式碼 `Angular/custom-component-solution`.

### 后续步骤 {#next-steps}

[擴充核心元件](extend-component.md)  — 瞭解如何擴充要與AEM SPA編輯器搭配使用的現有核心元件。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。
