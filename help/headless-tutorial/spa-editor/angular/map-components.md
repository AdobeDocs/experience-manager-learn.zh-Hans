---
title: 將SPA元件對應至AEM元件 | AEM SPA編輯器和Angular快速入門
description: 瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓使用者在SPA SPA編輯器中對AEM元件進行動態更新，類似於傳統AEM編寫。
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2372'
ht-degree: 1%

---

# 將SPA元件對應至AEM元件 {#map-components}

瞭解如何使用AEM SPA Editor JS SDK將Angular元件對應至Adobe Experience Manager (AEM)元件。 元件對應可讓使用者在SPA SPA編輯器中對AEM元件進行動態更新，類似於傳統AEM編寫。

本章深入探討AEM JSON模型API，以及如何將AEM元件公開的JSON內容作為prop自動插入Angular元件。

## 目标

1. 瞭解如何將AEM元件對應至SPA元件。
2. 瞭解兩者之間的差異 **容器** 元件和 **內容** 元件。
3. 建立對應至現有AEM元件的新Angular元件。

## 您將建置的內容

本章將檢查提供的如何 `Text` SPA元件已對應至AEM `Text`元件。 新 `Image` SPA元件是建立在SPA中使用以及在AEM中編寫的。 開箱即用的 **配置容器** 和 **範本編輯器** 原則也可用來建立外觀稍有變化的檢視。

![章節範例最終製作](./assets/map-components/final-page.png)

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. 使用Maven將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   若使用 [AEM 6.x](overview.md#compatibility) 新增 `classic` 設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或切換至分支以在本機簽出程式碼 `Angular/map-components-solution`.

## 對應方法

基本概念是對應SPA元件至AEM元件。 AEM元件、執行伺服器端、將內容匯出為JSON模型API的一部分。 JSON內容會由SPA使用，在瀏覽器中執行使用者端。 SPA元件和AEM元件之間會建立1:1對應。

![將AEM元件對應至Angular元件的高階概觀](./assets/map-components/high-level-approach.png)

*將AEM元件對應至Angular元件的高階概觀*

## Inspect文字元件

此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 提供 `Text` 對應至AEM的元件 [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 以下範例說明 **內容** 元件，在其中呈現 *內容* 來自AEM。

讓我們看看元件的運作方式。

### Inspect JSON模型

1. 在跳入SPA程式碼之前，請務必瞭解AEM提供的JSON模型。 導覽至 [核心元件程式庫](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 並檢視文字元件的頁面。 核心元件庫提供所有AEM核心元件的範例。
2. 選取 **JSON** 標籤以取得下列其中一個範例：

   ![文字JSON模型](./assets/map-components/text-json.png)

   您應該會看到三個屬性： `text`， `richText`、和 `:type`.

   `:type` 是保留屬性，其中列出 `sling:resourceType` AEM （或路徑）。 的值 `:type` 是用來將AEM元件對應至SPA元件的專案。

   `text` 和 `richText` 是公開給SPA元件的其他屬性。

### Inspect文字元件

1. 開啟新終端機，並導覽至 `ui.frontend` 檔案夾。 執行 `npm install` 然後 `npm start` 以啟動 **webpack開發伺服器**：

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   此 `ui.frontend` 模組目前設定為使用 [模擬JSON模型](./integrate-spa.md#mock-json).

2. 您應該會看到新的瀏覽器視窗開啟 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![含有模擬內容的Webpack開發伺服器](assets/map-components/initial-start.png)

3. 在您選擇的IDE中，開啟WKND SPA的AEM專案。 展開 `ui.frontend` 模組並開啟檔案 **text.component.ts** 在 `ui.frontend/src/app/components/text/text.component.ts`：

   ![Text.jsAngular元件原始碼](assets/map-components/vscode-ide-text-js.png)

4. 第一個要檢查的區域是 `class TextComponent` 於第35行：

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) Decorator可用來宣告欄位，這些欄位的值是透過對應之JSON物件設定的，之前已加以檢閱。

   `@HostBinding('innerHtml') get content()` 是公開所編寫文字內容的方法，其值來自 `this.text`. 在內容為RTF文字的情況下(由 `this.richText` flag)會略過Angular的內建安全性。 angular的 [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) 用於「清除」原始HTML並防止跨網站指令碼漏洞。 方法已繫結至 `innerHtml` 屬性使用 [@HostBinding](https://angular.io/api/core/HostBinding) 裝飾者。

5. 接下來檢查 `TextEditConfig` 於第24行：

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上述程式碼負責決定何時在AEM製作環境中呈現預留位置。 如果 `isEmpty` 方法傳回 **true** 然後會呈現預留位置。

6. 最後，檢視 `MapTo` 呼叫第53行：

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **對應至** 由AEM SPA編輯器JS SDK提供(`@adobe/cq-angular-editable-components`)。 路徑 `wknd-spa-angular/components/text` 代表 `sling:resourceType` AEM元件的URL。 此路徑會與 `:type` 已由先前觀察到的JSON模型公開。 **對應至** 剖析JSON模型回應並將正確的值傳遞至 `@Input()` SPA元件的變數。

   您可以找到AEM `Text` 元件定義於 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. 藉由修改 **en.model.json** 檔案位於 `ui.frontend/src/mocks/json/en.model.json`.

   在~line 62更新第一個 `Text` 值以使用 **`H1`** 和 **`u`** 標籤：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   返回瀏覽器以檢視 **webpack開發伺服器**：

   ![更新的文字模型](assets/map-components/updated-text-model.png)

   嘗試切換 `richText` 屬性介於 **true** / **false** 以檢視執行中的轉譯器邏輯。

8. Inspect **text.component.html** 於 `ui.frontend/src/app/components/text/text.component.html`.

   此檔案是空的，因為元件的整個內容是由 `innerHTML` 屬性。

9. Inspect **app.module.ts** 於 `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   此 **文字元件** 並未明確納入，而是透過以下方式動態納入 **AEMResponsiveGridComponent** 由AEM SPA編輯器JS SDK提供。 因此，必須列於 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 陣列。

## 建立影像元件

接下來，建立 `Image` 對應至AEM的Angular元件 [影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 此 `Image` 元件是 **內容** 元件。

### INSPECT JSON

在跳入SPA程式碼之前，請檢查AEM提供的JSON模型。

1. 導覽至 [核心元件庫中的影像範例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![影像核心元件JSON](./assets/map-components/image-json.png)

   屬性： `src`， `alt`、和 `title` 用於填入SPA `Image` 元件。

   >[!NOTE]
   >
   > 公開其他影像屬性(`lazyEnabled`， `widths`)可讓開發人員建立最適化和延遲載入元件。 本教學課程中建置的元件非常簡單，而且可以 **not** 使用這些進階屬性。

2. 返回IDE並開啟 `en.model.json` 於 `ui.frontend/src/mocks/json/en.model.json`. 由於這是我們專案的全新元件，因此我們需要「模擬」影像JSON。

   在~line 70，為新增一個JSON專案 `image` 模型（別忘了結尾逗號） `,` 在秒數之後 `text_386303036`)並更新 `:itemsOrder` 陣列。

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   專案包含位於的範例影像 `/mock-content/adobestock-140634652.jpeg` 搭配 **webpack開發伺服器**.

   您可以檢視完整的 [en.model.json此處](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. 新增元件要顯示的庫存像片。

   建立名為的新資料夾 **影像** 下 `ui.frontend/src/mocks`. 下載 [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) 並將其放在新建立的 **影像** 資料夾。 如有需要，請隨意使用您自己的影像。

### 實作影像元件

1. 停止 **webpack開發伺服器** 若已啟動。
2. 執行AngularCLI以建立新的影像元件 `ng generate component` 命令來自內部 `ui.frontend` 資料夾：

   ```shell
   $ ng generate component components/image
   ```

3. 在IDE中，開啟 **image.component.ts** 於 `ui.frontend/src/app/components/image/image.component.ts` 並依照以下方式更新：

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` 是判斷是否在AEM中轉譯作者預留位置的設定，根據 `src` 屬性已填入。

   `@Input()` 之 `src`， `alt`、和 `title` 是從JSON API對應的屬性。

   `hasImage()` 是判斷是否應轉譯影像的方法。

   `MapTo` 將SPA元件對應至位於「 」的AEM元件 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. 開啟 **image.component.html** 並依照以下方式更新：

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   這將呈現 `<img>` 元素if `hasImage` 傳回 **true**.

5. 開啟 **image.component.scss** 並依照以下方式更新：

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > 此 `:host-context` 規則是 **關鍵** AEM SPA編輯器預留位置才能正常運作。 打算在SPA頁面編輯器中編寫的所有AEM元件至少都需要此規則。

6. 開啟 `app.module.ts` 並新增 `ImageComponent` 至 `entryComponents` 陣列：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   喜歡 `TextComponent`，則 `ImageComponent` 動態載入，且必須包含在 `entryComponents` 陣列。

7. 開始 **webpack開發伺服器** 以檢視 `ImageComponent` 轉譯。

   ```shell
   $ npm run start:mock
   ```

   ![新增至模擬的影像](assets/map-components/image-added-mock.png)

   *影像已新增至SPA*

   >[!NOTE]
   >
   > **額外挑戰**：實作新方法來顯示值 `title` 做為影像下方的標題。

## 更新AEM中的原則

此 `ImageComponent` 元件僅在 **webpack開發伺服器**. 接下來，將更新的SPA部署至AEM並更新範本原則。

1. 停止 **webpack開發伺服器** 和從 **根** 在專案中，使用您的Maven技能將變更部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 從AEM開始畫面瀏覽至 **[!UICONTROL 工具]** > **[!UICONTROL 範本]** > **[WKND SPAANGULAR](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   選取並編輯 **SPA頁面**：

   ![編輯SPA頁面範本](assets/map-components/edit-spa-page-template.png)

3. 選取 **配置容器** 並按一下 **原則** 圖示以編輯原則：

   ![配置容器原則](./assets/map-components/layout-container-policy.png)

4. 下 **允許的元件** > **WKND SPAAngular — 內容** >檢查 **影像** 元件：

   ![已選取影像元件](assets/map-components/check-image-component.png)

   下 **預設元件** > **新增對應** 並選擇 **影像 — WKND SPAAngular — 內容** 元件：

   ![設定預設元件](assets/map-components/default-components.png)

   輸入 **mime型別** 之 `image/*`.

   按一下 **完成** 以儲存原則更新。

5. 在 **配置容器** 按一下 **原則** 圖示 **文字** 元件：

   ![文字元件原則圖示](./assets/map-components/edit-text-policy.png)

   建立名為的新原則 **WKND SPA文字**. 下 **外掛程式** > **格式設定** >勾選所有方塊以啟用其他格式選項：

   ![啟用RTE格式](assets/map-components/enable-formatting-rte.png)

   下 **外掛程式** > **段落樣式** >勾選方塊以 **啟用段落樣式**：

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   按一下 **完成** 以儲存原則更新。

6. 導覽至 **首頁** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   您也應該能夠編輯 `Text` 元件並新增其他段落樣式 **全熒幕** 模式。

   ![全熒幕RTF編輯](assets/map-components/full-screen-rte.png)

7. 您也應該能夠從以下位置拖放影像： **資產尋找器**：

   ![拖放影像](./assets/map-components/drag-drop-image.gif)

8. 透過新增您自己的影像 [AEM Assets](http://localhost:4502/assets.html/content/dam) 或安裝完成的標準程式碼基底 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest). 此 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 包含可在WKND SPA上重複使用的許多影像。 套件可使用以下方式安裝： [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp).

   ![封裝管理員安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect配置容器

支援 **配置容器** 由AEM SPA Editor SDK自動提供。 此 **配置容器**&#x200B;如名稱所示，是 **容器** 元件。 容器元件是接受JSON結構的元件，表示 *其他* 元件並動態例項化它們。

讓我們進一步檢查配置容器。

1. 在IDE中開啟 **responsive-grid.component.ts** 於 `ui.frontend/src/app/components/responsive-grid`：

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   此 `AEMResponsiveGridComponent` 實作為AEM SPA Editor SDK的一部分，並透過包含在專案中 `import-components`.

2. 在瀏覽器中導覽至 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON模型API — 回應式格線](./assets/map-components/responsive-grid-modeljson.png)

   此 **配置容器** 元件具有 `sling:resourceType` 之 `wcm/foundation/components/responsivegrid` 和，可由SPA編輯器透過以下路徑識別： `:type` 屬性，就像 `Text` 和 `Image` 元件。

   與使用重新調整元件大小的功能相同 [版面模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 可透過SPA編輯器使用。

3. 返回至 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 新增其他 **影像** 元件，並嘗試使用 **版面** 選項：

   ![使用版面模式重新調整影像大小](./assets/map-components/responsive-grid-layout-change.gif)

4. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 並觀察 `columnClassNames` 做為JSON的一部分：

   ![欄類別名稱](./assets/map-components/responsive-grid-classnames.png)

   類別名稱 `aem-GridColumn--default--4` 表示元件應以12欄格線為4欄寬。 更多有關「 」的詳細資訊 [可在此處找到回應式格線](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. 返回IDE並在 `ui.apps` 模組有一個使用者端程式庫定義於 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. 開啟檔案 `less/grid.less`.

   此檔案會決定中斷點(`default`， `tablet`、和 `phone`)用於 **配置容器**. 此檔案旨在根據專案規格自訂。 目前中斷點設定為 `1200px` 和 `650px`.

6. 您應該能夠使用的回應式功能和更新的RTF原則 `Text` 元件以製作檢視，如下所示：

   ![章節範例最終製作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何將SPA元件對應至AEM元件，並實作新的 `Image` 元件。 您也有機會探索的回應式功能 **配置容器**.

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或切換至分支以在本機簽出程式碼 `Angular/map-components-solution`.

### 后续步骤 {#next-steps}

[導覽與路由](navigation-routing.md)  — 瞭解如何使用SPA編輯器SDK將對應到AEM頁面，以支援SPA中的多個檢視。 動態導覽是使用Angular路由器實作，並新增至現有的Header元件。

## 額外優點 — 將設定保留至原始檔控制 {#bonus}

在許多情況下，尤其是在AEM專案開始時，將設定（例如範本和相關內容原則）保留到原始檔控制中很有價值。 這可確保所有開發人員都針對相同的內容和設定集，並可確保環境之間的額外一致性。 一旦專案達到一定的成熟度，管理範本的實務就可以交給一組特殊的超級使用者。

接下來的幾個步驟將使用Visual Studio Code IDE和 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 但可以使用任何工具和您已設定的IDE **提取** 或 **匯入** 來自AEM本機執行個體的內容。

1. 在Visual Studio Code IDE中，確定您已 **VSCode AEM Sync** 透過Marketplace擴充功能安裝：

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. 展開 **ui.content** 模組，並導覽至 `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **按一下滑鼠右鍵** 此 `templates` 資料夾並選取 **從AEM伺服器匯入**：

   ![VSCode匯入範本](assets/map-components/import-aem-servervscode.png)

4. 重複步驟以匯入內容，但選取 **原則** 資料夾位於 `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect `filter.xml` 檔案位於 `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   此 `filter.xml` file負責識別隨套件安裝的節點路徑。 請注意 `mode="merge"` 在表示現有內容將不會被修改的每個篩選器上，只會新增新內容。 由於內容作者可能正在更新這些路徑，因此程式碼部署必須更新 **not** 覆寫內容。 請參閱 [FileVault檔案](https://jackrabbit.apache.org/filevault/filter.html) 以取得有關使用篩選元素的詳細資訊。

   比較 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 以瞭解由每個模組管理的不同節點。
