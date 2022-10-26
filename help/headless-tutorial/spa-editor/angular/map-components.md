---
title: 将SPA组件映射到AEM组件 | AEM SPA Editor和Angular快速入门
description: 了解如何使用AEM SPA Editor JS SDK将Angular组件映射到Adobe Experience Manager(AEM)组件。 组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
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

# 将SPA组件映射到AEM组件 {#map-components}

了解如何使用AEM SPA Editor JS SDK将Angular组件映射到Adobe Experience Manager(AEM)组件。 组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。

本章更深入地介绍了AEM JSON模型API，以及如何将AEM组件公开的JSON内容自动作为prop注入到Angular组件中。

## 目标

1. 了解如何将AEM组件映射到SPA组件。
2. 了解 **容器** 组件和 **内容** 组件。
3. 创建新的Angular组件，以映射到现有AEM组件。

## 将构建的内容

本章将检查提供的 `Text` SPA组件已映射到AEM `Text`组件。 新 `Image` SPA组件已创建，可在SPA中使用并在AEM中创作。 的开箱即用功能 **布局容器** 和 **模板编辑器** 还将使用策略来创建外观稍有不同的视图。

![章节示例最终创作](./assets/map-components/final-page.png)

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment).

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) 添加 `classic` 用户档案：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或通过切换到分支在本地检出代码 `Angular/map-components-solution`.

## 映射方法

基本概念是将SPA组件映射到AEM组件。 AEM组件、运行服务器端，将内容导出为JSON模型API的一部分。 SPA会使用JSON内容，并在浏览器中运行客户端。 将创建SPA组件与AEM组件之间的1:1映射。

![将AEM组件映射到Angular组件的高级概述](./assets/map-components/high-level-approach.png)

*将AEM组件映射到Angular组件的高级概述*

## Inspect文本组件

的 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 提供 `Text` 映射到AEM的组件 [文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 这是 **内容** 组件，在其中，它呈现 *内容* 从AEM。

让我们看看组件的工作方式。

### Inspect JSON模型

1. 在跳转到SPA代码之前，请务必了解AEM提供的JSON模型。 导航到 [核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 并查看文本组件的页面。 核心组件库提供了所有AEM核心组件的示例。
2. 选择 **JSON** 选项卡，以查看以下示例：

   ![文本JSON模型](./assets/map-components/text-json.png)

   您应会看到三个资产： `text`, `richText`和 `:type`.

   `:type` 是一个保留属性，其中列出了 `sling:resourceType` （或路径）。 的值 `:type` 用于将AEM组件映射到SPA组件。

   `text` 和 `richText` 是显示给SPA组件的其他属性。

### Inspect文本组件

1. 打开新终端并导航到 `ui.frontend` 文件夹。 运行 `npm install` 然后 `npm start` 开始 **WebPack开发服务器**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   的 `ui.frontend` 模块当前设置为使用 [模拟JSON模型](./integrate-spa.md#mock-json).

2. 您应会看到一个新的浏览器窗口，该窗口将打开 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![包含模拟内容的WebPack开发服务器](assets/map-components/initial-start.png)

3. 在选择的IDE中，打开WKND SPA的AEM项目。 展开 `ui.frontend` 模块并打开文件 **text.component.ts** 在 `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.jsAngular组件源代码](assets/map-components/vscode-ide-text-js.png)

4. 首先要检查的是 `class TextComponent` 在第35行：

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

   [@Input()](https://angular.io/api/core/Input) decorator用于声明通过映射的JSON对象设置的值的字段，这些值在之前已审阅。

   `@HostBinding('innerHtml') get content()` 是一种方法，用于从的值公开创作的文本内容 `this.text`. 如果内容是富文本(由 `this.richText` 标记)会绕过Angular的内置安全性。 Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) 用于“清除”原始HTML并防止跨站点脚本漏洞。 方法绑定到 `innerHtml` 属性 [@HostBinding](https://angular.io/api/core/HostBinding) 装饰师。

5. 接下来，检查 `TextEditConfig` 在第24行：

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   上述代码负责确定何时在AEM创作环境中渲染占位符。 如果 `isEmpty` 方法返回 **true** 然后，会呈现占位符。

6. 最后看看 `MapTo` 致电~53线：

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** 由AEM SPA Editor JS SDK提供(`@adobe/cq-angular-editable-components`)。 路径 `wknd-spa-angular/components/text` 表示 `sling:resourceType` 的AEM组件。 此路径与 `:type` 由之前观察到的JSON模型公开。 **MapTo** 解析JSON模型响应，并将正确的值传递到 `@Input()` SPA组件的变量。

   您可以找到AEM `Text` 组件定义 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. 通过修改 **en.model.json** 文件位置 `ui.frontend/src/mocks/json/en.model.json`.

   第1次~第62行更新 `Text` 值 **`H1`** 和 **`u`** 标记：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   返回浏览器，查看 **WebPack开发服务器**:

   ![更新了文本模型](assets/map-components/updated-text-model.png)

   尝试切换 `richText` 属性介于 **true** / **false** ，以查看正在操作的渲染逻辑。

8. Inspect **text.component.html** at `ui.frontend/src/app/components/text/text.component.html`.

   此文件为空，因为组件的整个内容由 `innerHTML` 属性。

9. Inspect **app.module.ts** at `ui.frontend/src/app/app.module.ts`.

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

   的 **文本组件** 未明确包含，而是通过动态 **AEMResponsiveGridComponent** 由AEM SPA Editor JS SDK提供。 因此，必须在 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 数组。

## 创建图像组件

接下来，创建 `Image` Angular组件，已映射到AEM [图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 的 `Image` 组件是 **内容** 组件。

### Inspect JSON

在跳转到SPA代码之前，请检查AEM提供的JSON模型。

1. 导航到 [核心组件库中的图像示例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![图像核心组件JSON](./assets/map-components/image-json.png)

   属性 `src`, `alt`和 `title` 用于填充SPA `Image` 组件。

   >[!NOTE]
   >
   > 还显示了其他图像属性(`lazyEnabled`, `widths`)来创建自适应和延迟加载组件。 本教程中构建的组件非常简单，但是可以 **not** 使用这些高级属性。

2. 返回到IDE并打开 `en.model.json` at `ui.frontend/src/mocks/json/en.model.json`. 由于这是我们项目的新组件，因此我们需要“模拟”图像JSON。

   在第70行，为 `image` 模型(不要忘记尾随的逗号 `,` 在 `text_386303036`)并更新 `:itemsOrder` 数组。

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

   该项目包含一个位于 `/mock-content/adobestock-140634652.jpeg` 与 **WebPack开发服务器**.

   您可以查看完整 [en.model.json，此处为](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. 添加要由组件显示的库存照片。

   创建名为的新文件夹 **图像** 下 `ui.frontend/src/mocks`. 下载 [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) 然后放在新创建的 **图像** 文件夹。 如有需要，请随时使用您自己的图像。

### 实施图像组件

1. 停止 **WebPack开发服务器** 。
2. 通过运行AngularCLI创建新的映像组件 `ng generate component` 从内 `ui.frontend` 文件夹：

   ```shell
   $ ng generate component components/image
   ```

3. 在IDE中，打开 **image.component.ts** at `ui.frontend/src/app/components/image/image.component.ts` 更新如下：

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

   `ImageEditConfig` 是用于确定是否在AEM中渲染创作占位符的配置，具体取决于 `src` 属性。

   `@Input()` of `src`, `alt`和 `title` 是从JSON API映射的属性。

   `hasImage()` 是一种方法，用于确定是否应渲染图像。

   `MapTo` 将SPA组件映射到位于 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. 打开 **image.component.html** 并按以下方式更新：

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   这将呈现 `<img>` 元素 `hasImage` 返回 **true**.

5. 打开 **image.component.scss** 并按以下方式更新：

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
   > 的 `:host-context` 规则 **关键** 以使AEM SPA编辑器占位符正常运行。 要在AEM页面编辑器中创作的所有SPA组件都至少需要此规则。

6. 打开 `app.module.ts` 并添加 `ImageComponent` 到 `entryComponents` 数组：

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   如 `TextComponent`, `ImageComponent` 是动态加载的，且必须包含在 `entryComponents` 数组。

7. 启动 **WebPack开发服务器** 查看 `ImageComponent` 呈现。

   ```shell
   $ npm run start:mock
   ```

   ![模拟中添加了图像](assets/map-components/image-added-mock.png)

   *图像已添加到SPA*

   >[!NOTE]
   >
   > **奖金挑战**:实施新的方法来显示 `title` 图像下方的标题。

## 在AEM中更新策略

的 `ImageComponent` 组件仅在 **WebPack开发服务器**. 接下来，将更新的SPA部署到AEM并更新模板策略。

1. 停止 **WebPack开发服务器** 和 **根** 在项目中，使用您的Maven技能部署对AEM所做的更改：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 从AEM开始屏幕中，导航到 **[!UICONTROL 工具]** > **[!UICONTROL 模板]** > **[WKND SPAAngular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   选择并编辑 **SPA页面**:

   ![编辑SPA页面模板](assets/map-components/edit-spa-page-template.png)

3. 选择 **布局容器** 点击 **策略** 图标以编辑策略：

   ![布局容器策略](./assets/map-components/layout-container-policy.png)

4. 在 **允许的组件** > **WKND SPAAngular — 内容** >检查 **图像** 组件：

   ![已选择图像组件](assets/map-components/check-image-component.png)

   在 **默认组件** > **添加映射** 然后选择 **图像 — WKND SPAAngular — 内容** 组件：

   ![设置默认组件](assets/map-components/default-components.png)

   输入 **mime类型** of `image/*`.

   单击 **完成** 以保存策略更新。

5. 在 **布局容器** 单击 **策略** 图标 **文本** 组件：

   ![文本组件策略图标](./assets/map-components/edit-text-policy.png)

   创建名为的新策略 **WKND SPA文本**. 在 **插件** > **格式** >选中所有框以启用其他格式选项：

   ![启用RTE格式](assets/map-components/enable-formatting-rte.png)

   在 **插件** > **段落样式** >选中复选框 **启用段落样式**:

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   单击 **完成** 以保存策略更新。

6. 导航到 **主页** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   您还应该能够编辑 `Text` 组件和在 **全屏** 模式。

   ![全屏富文本编辑](assets/map-components/full-screen-rte.png)

7. 您还应该能够从 **资产查找器**:

   ![拖放图像](./assets/map-components/drag-drop-image.gif)

8. 通过 [AEM Assets](http://localhost:4502/assets.html/content/dam) 或为标准 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest). 的 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 包含许多可在WKND SPA中重复使用的图像。 可以使用 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect布局容器

支持 **布局容器** 由AEM SPA Editor SDK自动提供。 的 **布局容器**，如名称所示，为 **容器** 组件。 容器组件是接受JSON结构(表示 *其他* 组件，并动态实例化它们。

让我们进一步检查布局容器。

1. 在IDE打开中 **responsive-grid.component.ts** at `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   的 `AEMResponsiveGridComponent` 作为AEM SPA Editor SDK的一部分实施，并通过包含在项目中 `import-components`.

2. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON模型API — 响应式网格](./assets/map-components/responsive-grid-modeljson.png)

   的 **布局容器** 组件具有 `sling:resourceType` of `wcm/foundation/components/responsivegrid` 且由SPA编辑器使用 `:type` 资产，就像 `Text` 和 `Image` 组件。

   与使用 [布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 在SPA编辑器中可用。

3. 返回 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 添加其他 **图像** 并尝试使用 **布局** 选项：

   ![使用布局模式重新调整图像大小](./assets/map-components/responsive-grid-layout-change.gif)

4. 重新打开JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 观察 `columnClassNames` 作为JSON的一部分：

   ![列类名称](./assets/map-components/responsive-grid-classnames.png)

   类名称 `aem-GridColumn--default--4` 指示组件应宽4列（基于12列网格）。 有关 [可在此处找到响应式网格](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. 返回到IDE和 `ui.apps` 模块中有一个在 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. 打开文件 `less/grid.less`.

   此文件确定断点(`default`, `tablet`和 `phone`) **布局容器**. 此文件将根据项目规范进行自定义。 当前断点设置为 `1200px` 和 `650px`.

6. 您应该能够使用 `Text` 组件来创作视图，如下所示：

   ![章节示例最终创作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您学习了如何将SPA组件映射到AEM组件，并且还实施了 `Image` 组件。 您还有机会探索 **布局容器**.

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 或通过切换到分支在本地检出代码 `Angular/map-components-solution`.

### 后续步骤 {#next-steps}

[导航和路由](navigation-routing.md)  — 了解如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面来支持这些视图。 动态导航是使用Angular路由器实现的，并添加到现有的标头组件中。

## 附加练习 — 保留用于源代码控制的配置 {#bonus}

在很多情况下，特别是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源控制中非常有价值。 这可确保所有开发人员针对同一组内容和配置开展工作，并可确保各环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的做法就可以交给一组特定的高级用户。

接下来的几个步骤将使用Visual Studio代码IDE和 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 但可以使用您配置的任何工具和IDE **提取** 或 **导入** 来自AEM本地实例的内容。

1. 在Visual Studio代码IDE中，确保 **VSCode AEM同步** 通过Marketplace扩展安装：

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展开 **ui.content** 模块，并导航到 `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **右键单击** the `templates` 文件夹，选择 **从AEM Server导入**:

   ![VSCode导入模板](assets/map-components/import-aem-servervscode.png)

4. 重复导入内容的步骤，但选择 **策略** 位于 `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect `filter.xml` 位于 `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   的 `filter.xml` 文件负责标识随包一起安装的节点的路径。 请注意 `mode="merge"` 在指示不会修改现有内容的每个过滤器上，只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署必须执行以下操作 **not** 覆盖内容。 请参阅 [FileVault文档](https://jackrabbit.apache.org/filevault/filter.html) 有关使用过滤器元素的更多详细信息。

   比较 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 以了解每个模块管理的不同节点。
