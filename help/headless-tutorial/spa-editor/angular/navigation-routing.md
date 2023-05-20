---
title: 新增導覽和路由 | AEM SPA編輯器和Angular快速入門
description: 瞭解如何使用SPA頁面和AEM Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的標頭元件。
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2712'
ht-degree: 1%

---

# 新增導覽和路由 {#navigation-routing}

瞭解如何使用SPA頁面和AEM Editor SDK支援SPA中的多個檢視。 動態導覽是使用Angular路由實作，並新增至現有的標頭元件。

## 目标

1. 瞭解使用SPA編輯器時可用的SPA模型路由選項。
2. 瞭解如何使用 [angular路由](https://angular.io/guide/router) 以在SPA的不同檢視之間導覽。
3. 實作由AEM頁面階層驅動的動態導覽。

## 您將建置的內容

本章將導覽功能表新增至現有的 `Header` 元件。 導覽功能表是由AEM頁面階層所驅動，並使用 [導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
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

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或切換至分支以在本機簽出程式碼 `Angular/navigation-routing-solution`.

## Inspect HeaderComponent更新 {#inspect-header}

在前幾章中， `HeaderComponent` 元件是透過新增為純Angular元件 `app.component.html`. 在本章中， `HeaderComponent` 元件會從應用程式中移除，並透過 [範本編輯器](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=zh-Hans). 這可讓使用者設定 `HeaderComponent` 從AEM中。

>[!NOTE]
>
> 程式碼庫已進行數個CSS和JavaScript更新，以便開始本章節。 專注於核心概念，而非 **全部** 將會討論程式碼變更的專案。 您可以檢視完整變更 [此處](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. 在您選擇的IDE中，開啟本章的SPA入門專案。
2. 在 `ui.frontend` 模組檢查檔案 `header.component.ts` 於： `ui.frontend/src/app/components/header/header.component.ts`.

   已進行數個更新，包括新增 `HeaderEditConfig` 和 `MapTo` 啟用元件對應至AEM元件的方式 `wknd-spa-angular/components/header`.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   請注意 `@Input()` 註解 `items`. `items` 將包含從AEM傳入的導覽物件陣列。

3. 在 `ui.apps` 模組檢查AEM的元件定義 `Header` 元件： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` 元件將繼承的所有功能 [導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) 透過 `sling:resourceSuperType` 屬性。

## 將HeaderComponent新增至SPA範本 {#add-header-template}

1. 開啟瀏覽器並登入AEM， [http://localhost:4502/](http://localhost:4502/). 起始程式碼基底應已部署。
2. 導覽至 **[!UICONTROL SPA頁面範本]**： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 選取最外層 **[!UICONTROL 根配置容器]** 並按一下其 **[!UICONTROL 原則]** 圖示。 請小心 **not** 以選取 **[!UICONTROL 配置容器]** 已解除製作鎖定。

   ![選取根配置容器原則圖示](assets/navigation-routing/root-layout-container-policy.png)

4. 複製目前的原則並建立名為的新原則 **[!UICONTROL SPA結構]**：

   ![SPA結構原則](assets/map-components/spa-policy-update.png)

   下 **[!UICONTROL 允許的元件]** > **[!UICONTROL 一般]** >選取 **[!UICONTROL 配置容器]** 元件。

   下 **[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPAANGULAR — 結構]** >選取 **[!UICONTROL 頁首]** 元件：

   ![選取標頭元件](assets/map-components/select-header-component.png)

   下 **[!UICONTROL 允許的元件]** > **[!UICONTROL WKND SPAANGULAR — 內容]** >選取 **[!UICONTROL 影像]** 和 **[!UICONTROL 文字]** 元件。 您總共應該選取4個元件。

   单击&#x200B;**[!UICONTROL 完成]**&#x200B;以保存更改。

5. **重新整理** 頁面。 新增 **[!UICONTROL 頁首]** 元件在解鎖前面 **[!UICONTROL 配置容器]**：

   ![將標頭元件新增至範本](./assets/navigation-routing/add-header-component.gif)

6. 選取 **[!UICONTROL 頁首]** 元件並按一下其 **原則** 圖示來編輯原則。

   ![按一下標頭原則](assets/navigation-routing/header-policy-icon.png)

7. 使用建立新原則 **[!UICONTROL 原則標題]** 之 **&quot;WKND SPA Header&quot;**.

   在 **[!UICONTROL 屬性]**：

   * 設定 **[!UICONTROL 導覽根目錄]** 至 `/content/wknd-spa-angular/us/en`.
   * 設定 **[!UICONTROL 排除根層級]** 至 **1**.
   * 取消核取 **[!UICONTROL 收集所有子頁面]**.
   * 設定 **[!UICONTROL 導覽結構深度]** 至 **3**.

   ![設定標頭原則](assets/navigation-routing/header-policy.png)

   這會收集下方2個層級的導覽 `/content/wknd-spa-angular/us/en`.

8. 儲存變更後，您應會看到填入的 `Header` 做為範本的一部分：

   ![填入的標頭元件](assets/navigation-routing/populated-header.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，這些頁面將用作SPA中的不同檢視。 我們也會檢查AEM所提供JSON模型的階層結構。

1. 導覽至 **網站** 主控台： [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). 選取 **wknd SPAAngular首頁** 並按一下 **[!UICONTROL 建立]** > **[!UICONTROL 頁面]**：

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

2. 下 **[!UICONTROL 範本]** 選取 **[!UICONTROL SPA頁面]**. 下 **[!UICONTROL 屬性]** 輸入 **&quot;第1頁&quot;** 的 **[!UICONTROL 標題]** 和 **&quot;page-1&quot;** 作為名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下 **[!UICONTROL 建立]** 而在對話方塊快顯視窗中，按一下 **[!UICONTROL 開啟]** 以在AEM SPA編輯器中開啟頁面。

3. 新增 **[!UICONTROL 文字]** 元件至主要 **[!UICONTROL 配置容器]**. 編輯元件並輸入文字： **&quot;第1頁&quot;** 使用RTE和 **H1** 元素（您必須進入全熒幕模式才能變更段落元素）

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

4. 返回AEM Sites主控台，並重複上述步驟，建立名為的第二個頁面 **「第2頁」** 作為同層級 **第1頁**. 新增內容至 **第2頁** 以便輕鬆識別。
5. 最後，建立第三個頁面， **「第3頁」** 但作為 **子項** 之 **第2頁**. 完成之後，網站階層應如下所示：

   ![範例網站階層](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新標籤中，開啟AEM提供的JSON模型API： [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). SPA首次載入時會要求此JSON內容。 外部結構如下所示：

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   下 `:children` 您應該會看到每個已建立頁面的專案。 所有頁面的內容都在此初始JSON請求中。 一旦導覽路由實作後，SPA的後續檢視就會快速載入，因為內容在使用者端已可供使用。

   載入是不明智的 **全部** 初始JSON請求中SPA內容的變數，因為這會減慢初始頁面載入的速度。 接下來，讓我們檢視如何收集頁面的階層式深度。

7. 導覽至 **SPA根目錄** 範本位於： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   按一下 **[!UICONTROL 頁面屬性功能表]** > **[!UICONTROL 頁面原則]**：

   ![開啟SPA Root的頁面原則](assets/navigation-routing/open-page-policy.png)

8. 此 **SPA根目錄** 範本有一個額外的 **[!UICONTROL 階層結構]** 索引標籤來控制收集的JSON內容。 此 **[!UICONTROL 結構深度]** 決定網站階層中要多深才能收集下方的子頁面 **根**. 您也可以使用 **[!UICONTROL 結構模式]** 根據規則運算式篩選掉其他頁面的欄位。

   更新 **[!UICONTROL 結構深度]** 至 **&quot;2&quot;**：

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下 **[!UICONTROL 完成]** 以儲存對原則的變更。

9. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   請注意 **第3頁** 路徑已移除： `/content/wknd-spa-angular/us/en/home/page-2/page-3` 來自初始JSON模型。

   稍後，我們將觀察AEM SPA Editor SDK如何動態載入其他內容。

## 實作導覽

接下來，使用新的來實作導覽功能表 `NavigationComponent`. 我們可以直接在中新增程式碼 `header.component.html` 但避免大型元件是較好的作法。 請改為實作 `NavigationComponent` 日後可能會重複使用。

1. 檢閱AEM公開的JSON `Header` 元件於 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)：

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   AEM頁面的階層性質以JSON建模，可用於填入導覽功能表。 記住 `Header` 元件會繼承 [導覽核心元件](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) 透過JSON公開的內容會自動對應至Angular `@Input` 註解。

2. 開啟新的終端機視窗並瀏覽至 `ui.frontend` SPA專案的資料夾。 建立新的 `NavigationComponent` 使用AngularCLI工具：

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 接下來，建立名為的類別 `NavigationLink` 在新建立的中使用AngularCLI `components/navigation` 目錄：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回您選擇的IDE並在以下位置開啟檔案： `navigation-link.ts` 於 `/src/app/components/navigation/navigation-link.ts`.

   ![開啟navigation-link.ts檔案](assets/navigation-routing/ide-navigation-link-file.png)

5. 填入 `navigation-link.ts` ，其功能如下：

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   這是一個簡單的類別，可代表個別導覽連結。 在類別建構函式中 `data` 作為從AEM傳入的JSON物件。 此類別同時用於 `NavigationComponent` 和 `HeaderComponent` 以輕鬆填入導覽結構。

   不會執行資料轉換，建立此類別主要是為了強式輸入JSON模型。 請注意 `this.children` 型別為 `NavigationLink[]` 而且建構函式會遞回建立 `NavigationLink` 中每個專案的物件 `children` 陣列。 召回的JSON模型 `Header` 為階層式。

6. 開啟檔案 `navigation-link.spec.ts`. 這是的測試檔案 `NavigationLink` 類別。 使用以下專案更新它：

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   請注意 `const data` 會遵循先前針對單一連結檢查的相同JSON模型。 這遠非強大的單位測試，但應該足以測試下列的建構函式： `NavigationLink`.

7. 開啟檔案 `navigation.component.ts`. 使用以下專案更新它：

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` 預期 `object[]` 已命名 `items` 這是來自AEM的JSON模型。 此類別會公開單一方法 `get navigationLinks()` 會傳回陣列 `NavigationLink` 物件。

8. 開啟檔案 `navigation.component.html` 並更新為下列專案：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   這會產生一個初始 `<ul>` 並呼叫 `get navigationLinks()` 方法來源 `navigation.component.ts`. 一個 `<ng-container>` 用於呼叫範本，範本名稱為 `recursiveListTmpl` 並傳遞給 `navigationLinks` 作為變數，命名為 `links`.

   新增 `recursiveListTmpl` 下一步：

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   此處會實作導覽連結的其餘演算。 請注意，變數 `link` 屬於型別 `NavigationLink` 以及該類別建立的所有方法/屬性皆可使用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) 已使用，而非正常 `href` 屬性。 這可讓我們連結至應用程式中的特定路由，不需要重新整理整頁。

   導覽的遞回部分也透過建立另一個來實作 `<ul>` 如果目前 `link` 具有非空白 `children` 陣列。

9. 更新 `navigation.component.spec.ts` 新增支援 `RouterTestingModule`：

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   新增 `RouterTestingModule` 為必要，因為元件使用 `[routerLink]`.

10. 更新 `navigation.component.scss` 將一些基本樣式新增至 `NavigationComponent`：

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## 更新標頭元件

現在， `NavigationComponent` 「 」已實作 `HeaderComponent` 必須更新才能參考它。

1. 開啟終端機並導覽至 `ui.frontend` SPA專案中的資料夾。 開始 **webpack開發伺服器**：

   ```shell
   $ npm start
   ```

2. 開啟瀏覽器索引標籤並導覽至 [http://localhost:4200/](http://localhost:4200/).

   此 **webpack開發伺服器** 應設定為從AEM的本機執行個體(`ui.frontend/proxy.conf.json`)。 這可讓我們直接針對教學課程中先前內容在AEM中建立的內容進行編碼。

   ![功能表切換正在運作](./assets/navigation-routing/nav-toggle-static.gif)

   此 `HeaderComponent` 目前已經實作功能表切換功能。 接下來，新增導覽元件。

3. 返回您選擇的IDE並開啟檔案 `header.component.ts` 於 `ui.frontend/src/app/components/header/header.component.ts`.
4. 更新 `setHomePage()` 移除硬式編碼字串並使用AEM元件傳入之動態prop的方法：

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   的新執行個體 `NavigationLink` 建立依據 `items[0]`，是從AEM傳入的導覽JSON模型的根目錄。 `this.route.snapshot.data.path` 傳回目前Angular路由的路徑。 此值用於決定目前的路由是否為 **首頁**. `this.homePageUrl` 用於填入 **標誌**.

5. 開啟 `header.component.html` 和將導覽的靜態預留位置取代為新建立的參照 `NavigationComponent`：

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 屬性傳遞 `@Input() items` 從 `HeaderComponent` 至 `NavigationComponent` 建置導覽的位置。

6. 開啟 `header.component.spec.ts` 並為新增宣告 `NavigationComponent`：

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   由於 `NavigationComponent` 現在已用作 `HeaderComponent` 它需要宣告為測試平台的一部分。

7. 儲存對任何開啟檔案的變更並返回 **webpack開發伺服器**： [http://localhost:4200/](http://localhost:4200/)

   ![完成的標頭導覽](assets/navigation-routing/completed-header.png)

   按一下功能表切換來開啟導覽，您應該會看到填入的導覽連結。 您應該能夠導覽至SPA的不同檢視。

## 瞭解SPA路由

現在導覽已實作，請在AEM中檢查路由。

1. 在IDE中開啟檔案 `app-routing.module.ts` 於 `ui.frontend/src/app`.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   此 `routes: Routes = [];` array會定義Angular元件對應的路徑或導覽路徑。

   `AemPageMatcher` 是自訂Angular路由器 [UrlMatcher](https://angular.io/api/router/UrlMatcher)，會比對AEM中「看起來」屬於此Angular應用程式一部分的頁面的任何內容。

   `PageComponent` 是Angular元件，代表AEM中的頁面，用來呈現相符的路由。 此 `PageComponent` 稍後將在教學課程中檢閱。

   `AemPageDataResolver`由AEM SPA編輯器JS SDK提供，為自訂 [angular路由器解析程式](https://angular.io/api/router/Resolve) 用於將路由URL (AEM中的路徑，包括.html副檔名)轉換為AEM中的資源路徑（頁面路徑減去副檔名）。

   例如， `AemPageDataResolver` 轉換路由的URL `content/wknd-spa-angular/us/en/home.html` 到的路徑 `/content/wknd-spa-angular/us/en/home`. 用於根據JSON模型API中的路徑解析頁面內容。

   `AemPageRouteReuseStrategy`由AEM SPA編輯器JS SDK提供，為自訂 [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) 可防止重複使用 `PageComponent` 跨路由。 否則，導覽至頁面「B」時，可能會顯示頁面「A」的內容。

2. 開啟檔案 `page.component.ts` 於 `ui.frontend/src/app/components/page/`.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   此 `PageComponent` 處理從AEM擷取的JSON時需要，並作為呈現路由的Angular元件。

   `ActivatedRoute`(由Angular路由器模組提供)包含的狀態，指出應將哪些AEM頁面的JSON內容載入此Angular頁面元件例項。

   `ModelManagerService`，會根據路由取得JSON資料，並將資料對應至類別變數 `path`， `items`， `itemsOrder`. 這些資料隨後將傳遞至 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 開啟檔案 `page.component.html` 於 `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包含 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). 變數 `path`， `items`、和 `itemsOrder` 傳遞至 `AEMPageComponent`. 此 `AemPageComponent`，會透過SPA編輯器JavaScript SDK提供，接著會反複處理此資料，並根據JSON資料動態例項化Angular元件，如 [對應元件教學課程](./map-components.md).

   此 `PageComponent` 其實只是 `AEMPageComponent` 而且它是 `AEMPageComponent` 如此一來，大部分繁重的工作都會正確地將JSON模型對應至Angular元件。

## Inspect AEM中的SPA路由

1. 開啟終端機並停止 **webpack開發伺服器** 若已啟動。 導覽至專案的根目錄，然後使用您的Maven技能將專案部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular專案已啟用一些非常嚴格的連結規則。 如果Maven組建失敗，請檢查錯誤並尋找 **在列出的檔案中找到Lint錯誤。**. 修正Linter發現的任何問題，並重新執行Maven命令。

2. 導覽至AEM中的SPA首頁： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) 並開啟瀏覽器的開發人員工具。 以下熒幕擷取畫面是從Google Chrome瀏覽器擷取。

   重新整理頁面，您應該會看到XHR要求 `/content/wknd-spa-angular/us/en.model.json`，即SPA根目錄。 請注意，根據教學課程中先前進行的階層深度設定，SPA根範本僅包含三個子頁面。 這不包括 **第3頁**.

   ![初始JSON請求 — SPA根目錄](assets/navigation-routing/initial-json-request.png)

3. 在開發人員工具開啟的狀態下，導覽至 **第3頁**：

   ![第3頁導覽](assets/navigation-routing/page-three-navigation.png)

   請注意，已提出新的XHR要求給： `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR要求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理員瞭解 **第3頁** JSON內容無法使用，並自動觸發其他XHR請求。

4. 繼續使用各種導覽連結導覽SPA。 請注意，不會提出其他XHR要求，也不會發生完整頁面重新整理。 這可讓一般使用者快速使用SPA，並減少傳回AEM的不必要請求。

   ![已實作導覽](assets/navigation-routing/final-navigation-implemented.gif)

5. 透過直接導覽至以下位置，體驗深層連結： [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). 請注意，瀏覽器的後退按鈕仍會繼續運作。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何使用SPA編輯器SDK將對應到AEM頁面，以支援SPA中的多個檢視。 動態導覽已使用Angular路由實作，並新增至 `Header` 元件。

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或切換至分支以在本機簽出程式碼 `Angular/navigation-routing-solution`.

### 后续步骤 {#next-steps}

[建立自訂元件](custom-component.md)  — 瞭解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。
