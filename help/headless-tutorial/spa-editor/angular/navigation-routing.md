---
title: 添加导航和路由 | AEM SPA Editor和Angular快速入门
description: 了解如何使用SPA页面和SPA Editor SDK支持AEM中的多个视图。 动态导航是使用Angular路由实施的，并添加到现有的标题组件中。
sub-product: sites
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2712'
ht-degree: 0%

---

# 添加导航和路由 {#navigation-routing}

了解如何使用SPA页面和SPA Editor SDK支持AEM中的多个视图。 动态导航是使用Angular路由实施的，并添加到现有的标题组件中。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
2. 了解使用 [Angular路由](https://angular.io/guide/router) 可在SPA的不同视图之间导航。
3. 实施由AEM页面层次结构驱动的动态导航。

## 将构建的内容

本章将导航菜单添加到现有 `Header` 组件。 导航菜单由AEM页面层次结构驱动，并使用 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![已实施导航](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment).

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) 添加 `classic` 用户档案：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest). 提供的图像 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 在WKND SPA中重新使用。 可以使用 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或通过切换到分支在本地检出代码 `Angular/navigation-routing-solution`.

## Inspect HeaderComponent更新 {#inspect-header}

在前几章中， `HeaderComponent` 组件作为纯Angular组件添加，该组件通过 `app.component.html`. 在本章中， `HeaderComponent` 组件将从应用程序中删除，并通过 [模板编辑器](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). 这允许用户配置 `HeaderComponent` 从AEM。

>[!NOTE]
>
> 已对代码库进行了多次CSS和JavaScript更新，以开始本章。 重点关注核心概念，而不是 **全部** 将讨论代码更改。 您可以查看完整更改 [此处](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. 在您选择的IDE中，打开本章的SPA起始项目。
2. 在 `ui.frontend` 模块检查文件 `header.component.ts` at: `ui.frontend/src/app/components/header/header.component.ts`.

   已进行了一些更新，包括添加了 `HeaderEditConfig` 和 `MapTo` 启用组件以映射到AEM组件 `wknd-spa-angular/components/header`.

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

   请注意 `@Input()` 注释 `items`. `items` 将包含从AEM传入的导航对象数组。

3. 在 `ui.apps` 模块检查AEM的组件定义 `Header` 组件： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` 组件将继承 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) 通过 `sling:resourceSuperType` 属性。

## 将HeaderComponent添加到SPA模板 {#add-header-template}

1. 打开浏览器并登录AEM, [http://localhost:4502/](http://localhost:4502/). 应该已部署起始代码库。
2. 导航到 **[!UICONTROL SPA页面模板]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 选择最外侧 **[!UICONTROL 根布局容器]** 单击 **[!UICONTROL 策略]** 图标。 小心 **not** 选择 **[!UICONTROL 布局容器]** 未锁定进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

4. 复制当前策略并创建一个名为 **[!UICONTROL SPA结构]**:

   ![SPA结构策略](assets/map-components/spa-policy-update.png)

   在 **[!UICONTROL 允许的组件]** > **[!UICONTROL 常规]** >选择 **[!UICONTROL 布局容器]** 组件。

   在 **[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR — 结构]** >选择 **[!UICONTROL 标题]** 组件：

   ![选择标题组件](assets/map-components/select-header-component.png)

   在 **[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR — 内容]** >选择 **[!UICONTROL 图像]** 和 **[!UICONTROL 文本]** 组件。 您应选择总共4个组件。

   单击 **[!UICONTROL 完成]** 以保存更改。

5. **刷新页面。**&#x200B;添加 **[!UICONTROL 标题]** 未锁定组件上方的组件 **[!UICONTROL 布局容器]**:

   ![将标题组件添加到模板](./assets/navigation-routing/add-header-component.gif)

6. 选择 **[!UICONTROL 标题]** 组件，单击其 **策略** 图标以编辑策略。

   ![单击标题策略](assets/navigation-routing/header-policy-icon.png)

7. 使用 **[!UICONTROL 策略标题]** of **&quot;WKND SPA标头&quot;**.

   在 **[!UICONTROL 属性]**:

   * 设置 **[!UICONTROL 导航根]** to `/content/wknd-spa-angular/us/en`.
   * 设置 **[!UICONTROL 排除根级别]** to **1**.
   * 取消选中 **[!UICONTROL 收集所有子页面]**.
   * 设置 **[!UICONTROL 导航结构深度]** to **3**.

   ![配置标头策略](assets/navigation-routing/header-policy.png)

   这将收集下方2层的导航 `/content/wknd-spa-angular/us/en`.

8. 保存更改后，您应会看到填充的 `Header` 作为模板的一部分：

   ![填充的标题组件](assets/navigation-routing/populated-header.png)

## 创建子页面

接下来，在AEM中创建其他页面，以用作SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到 **站点** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). 选择 **WKND SPAAngular主页** 单击 **[!UICONTROL 创建]** > **[!UICONTROL 页面]**:

   ![创建新页面](assets/navigation-routing/create-new-page.png)

2. 在 **[!UICONTROL 模板]** 选择 **[!UICONTROL SPA页面]**. 在 **[!UICONTROL 属性]** enter **&quot;第1页&quot;** 对于 **[!UICONTROL 标题]** 和 **&quot;page-1&quot;** 作为名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击 **[!UICONTROL 创建]** 并在对话框弹出窗口中，单击 **[!UICONTROL 打开]** 以在AEM SPA编辑器中打开页面。

3. 添加新 **[!UICONTROL 文本]** 组件到主 **[!UICONTROL 布局容器]**. 编辑组件并输入文本： **&quot;第1页&quot;** 使用RTE和 **H1** 元素（您必须进入全屏模式才能更改段落元素）

   ![示例内容页面1](assets/navigation-routing/page-1-sample-content.png)

   请随时添加其他内容，如图像。

4. 返回到AEM Sites控制台并重复上述步骤，从而创建另一个名为 **&quot;第2页&quot;** 作为 **第1页**. 将内容添加到 **第2页** 以便易于识别。
5. 最后创建第三个页面， **&quot;第3页&quot;** 但作为 **孩子** of **第2页**. 完成网站层次结构后，应如下所示：

   ![网站层次结构示例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新选项卡中，打开由AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 首次加载SPA时，将请求此JSON内容。 外部结构如下所示：

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

   在 `:children` 您应会看到创建的每个页面对应的条目。 所有页面的内容都位于此初始JSON请求中。 一旦实施了导航路由，将快速加载SPA的后续视图，因为内容已在客户端可用。

   加载不明智 **全部** 的JSON请求中SPA内容的URL，因为这会减慢初始页面加载速度。 接下来，我们将看一看页面的层级深度是如何收集的。

7. 导航到 **SPA根** 模板： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   单击 **[!UICONTROL 页面属性菜单]** > **[!UICONTROL 页面策略]**:

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

8. 的 **SPA根** 模板具有额外的 **[!UICONTROL 分层结构]** 选项卡来控制收集的JSON内容。 的 **[!UICONTROL 结构深度]** 确定网站层级中收集子页面的深度 **根**. 您还可以使用 **[!UICONTROL 结构模式]** 字段，以根据正则表达式筛选其他页面。

   更新 **[!UICONTROL 结构深度]** to **&quot;2&quot;**:

   ![更新结构深度](assets/navigation-routing/update-structure-depth.png)

   单击 **[!UICONTROL 完成]** 以保存对策略所做的更改。

9. 重新打开JSON模型 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

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

   请注意， **第3页** 路径已删除： `/content/wknd-spa-angular/us/en/home/page-2/page-3` 从初始JSON模型。

   稍后，我们将观察AEM SPA Editor SDK如何动态加载其他内容。

## 实施导航

接下来，使用新的 `NavigationComponent`. 我们可以直接将代码添加到 `header.component.html` 但最好的做法是避免出现大型组件。 而是实施 `NavigationComponent` 以后可能会重新使用。

1. 查看由AEM公开的JSON `Header` 组件位置 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   AEM页面的层次结构是使用JSON建模的，可用于填充导航菜单。 记得 `Header` 组件会继承 [导航核心组件](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) 并且通过JSON公开的内容会自动映射到该Angular `@Input` 注释。

2. 打开新的终端窗口并导航到 `ui.frontend` 文件夹。 新建 `NavigationComponent` 使用AngularCLI工具：

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 接下来创建一个名为 `NavigationLink` 在新创建的Angular中使用CLI `components/navigation` 目录：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回到您选择的IDE，然后在 `navigation-link.ts` at `/src/app/components/navigation/navigation-link.ts`.

   ![打开navigation-link.ts文件](assets/navigation-routing/ide-navigation-link-file.png)

5. 填充 `navigation-link.ts` ，具有以下特点：

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

   这是一个用于表示单个导航链接的简单类。 在类构造函数中，我们期望 `data` 为从AEM传入的JSON对象。 此类在 `NavigationComponent` 和 `HeaderComponent` 以轻松填充导航结构。

   不执行任何数据转换，此类主要用于强烈键入JSON模型。 请注意 `this.children` 键入为 `NavigationLink[]` 构造函数递归地创建新 `NavigationLink` 对象 `children` 数组。 回想一下 `Header` 是分层的。

6. 打开文件 `navigation-link.spec.ts`. 这是 `NavigationLink` 类。 使用以下内容更新它：

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

   请注意 `const data` 遵循之前针对单个链接检查的相同JSON模型。 这远非一个可靠的单元测试，但它应该足以测试的构造函数 `NavigationLink`.

7. 打开文件 `navigation.component.ts`. 使用以下内容更新它：

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

   `NavigationComponent` 预期 `object[]` 已命名 `items` 即AEM中的JSON模型。 此类会公开单个方法 `get navigationLinks()` 返回 `NavigationLink` 对象。

8. 打开文件 `navigation.component.html` 并使用以下内容更新：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   这会生成一个初始 `<ul>` 并调用 `get navigationLinks()` 方法 `navigation.component.ts`. 安 `<ng-container>` 用于调用名为 `recursiveListTmpl` 然后传递 `navigationLinks` 作为名为的变量 `links`.

   添加 `recursiveListTmpl` 下一个：

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

   此处实现了导航链接的其余渲染。 请注意变量 `link` 是类型 `NavigationLink` 和由该类创建的所有方法/属性都可用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) 使用而不是正常 `href` 属性。 这样，我们便无需刷新整页，即可链接到应用程序中的特定路由。

   导航的递归部分也通过创建另一个 `<ul>` 如果当前 `link` 具有非空 `children` 数组。

9. 更新 `navigation.component.spec.ts` 添加对 `RouterTestingModule`:

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

   添加 `RouterTestingModule` 是必需的，因为组件使用 `[routerLink]`.

10. 更新 `navigation.component.scss` 向中添加一些基本样式 `NavigationComponent`:

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

## 更新标头组件

现在， `NavigationComponent` 已经实施， `HeaderComponent` 必须更新才能引用它。

1. 打开终端并导航到 `ui.frontend` 文件夹。 启动 **WebPack开发服务器**:

   ```shell
   $ npm start
   ```

2. 打开浏览器选项卡，然后导航到 [http://localhost:4200/](http://localhost:4200/).

   的 **WebPack开发服务器** 应该配置为从AEM的本地实例代理JSON模型(`ui.frontend/proxy.conf.json`)。 这将允许我们从教程前面的部分直接针对在AEM中创建的内容进行代码。

   ![菜单切换工作](./assets/navigation-routing/nav-toggle-static.gif)

   的 `HeaderComponent` 当前已实施菜单切换功能。 接下来，添加导航组件。

3. 返回到您选择的IDE，然后打开文件 `header.component.ts` at `ui.frontend/src/app/components/header/header.component.ts`.
4. 更新 `setHomePage()` 方法来删除硬编码的字符串，并使用由AEM组件传入的动态prop:

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

   的新实例 `NavigationLink` 创建依据 `items[0]`，从AEM传入的导航JSON模型的根。 `this.route.snapshot.data.path` 返回当前Angular路由的路径。 此值用于确定当前路由是否为 **主页**. `this.homePageUrl` 用于填充 **徽标**.

5. 打开 `header.component.html` 并将导航的静态占位符替换为对新创建的引用 `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 属性传递 `@Input() items` 从 `HeaderComponent` 到 `NavigationComponent` 在那里构建导航。

6. 打开 `header.component.spec.ts` 并为 `NavigationComponent`:

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

   自 `NavigationComponent` 现在用作 `HeaderComponent` 它需要声明为测试台的一部分。

7. 保存对任何打开的文件所做的更改并返回到 **WebPack开发服务器**: [http://localhost:4200/](http://localhost:4200/)

   ![已完成标题导航](assets/navigation-routing/completed-header.png)

   单击菜单切换打开导航，此时您应会看到填充的导航链接。 您应该能够导航到SPA的不同视图。

## 了解SPA路由

现在，导航已实施，请在AEM中检查路由。

1. 在IDE中，打开文件 `app-routing.module.ts` at `ui.frontend/src/app`.

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

   的 `routes: Routes = [];` 数组定义到Angular组件映射的路由或导航路径。

   `AemPageMatcher` 是自定义Angular路由器 [UrlMatcher](https://angular.io/api/router/UrlMatcher)，与AEM中属于此Angular应用程序一部分的任何“外观”页面相匹配。

   `PageComponent` 是AEM中表示页面的Angular组件，用于呈现匹配的路由。 的 `PageComponent` 稍后会在教程中进行审阅。

   `AemPageDataResolver`，由AEM SPA Editor JS SDK提供，是一个自定义 [Angular路由器解析程序](https://angular.io/api/router/Resolve) 用于将路由URL(AEM中的路径，包括.html扩展)转换为AEM中的资源路径（页面路径，即不包含扩展）。

   例如， `AemPageDataResolver` 转换路由的URL `content/wknd-spa-angular/us/en/home.html` 进入 `/content/wknd-spa-angular/us/en/home`. 用于根据JSON模型API中的路径解析页面内容。

   `AemPageRouteReuseStrategy`，由AEM SPA Editor JS SDK提供，是一个自定义 [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) 防止重复使用 `PageComponent` 跨路线。 否则，页面“A”中的内容可能会在导航到页面“B”时显示。

2. 打开文件 `page.component.ts` at `ui.frontend/src/app/components/page/`.

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

   的 `PageComponent` 需要处理从AEM检索到的JSON，并用作呈现路由的Angular组件。

   `ActivatedRoute`，由Angular路由器模块提供，包含状态，指示应将哪个AEM页面的JSON内容加载到此Angular页组件实例中。

   `ModelManagerService`，根据路由获取JSON数据并将该数据映射到类变量 `path`, `items`, `itemsOrder`. 然后，这些内容将传递到 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 打开文件 `page.component.html` at `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包括 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). 变量 `path`, `items`和 `itemsOrder` 被传递到 `AEMPageComponent`. 的 `AemPageComponent`，通过SPA编辑器提供的JavaScript SDK随后将迭代此数据，并根据( [映射组件教程](./map-components.md).

   的 `PageComponent` 只是代表 `AEMPageComponent` 而是 `AEMPageComponent` 这样可进行大部分繁重的操作，以将JSON模型正确映射到Angular组件。

## Inspect AEM中的SPA路由

1. 打开终端并停止 **WebPack开发服务器** 。 导航到项目的根，然后使用您的Maven技能将项目部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular项目启用了一些非常严格的链接规则。 如果Maven生成失败，请检查错误并查找 **在列出的文件中发现Lint错误。**. 修复了linter发现的任何问题，并重新运行Maven命令。

2. 导航到AEM中的SPA主页： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) 并打开浏览器的开发人员工具。 下面的屏幕截图是从Google Chrome浏览器中捕获的。

   刷新页面，此时您应会看到 `/content/wknd-spa-angular/us/en.model.json`，即SPA根。 请注意，根据本教程前面对SPA根模板进行的层次结构深度配置，只包含三个子页面。 这不包括 **第3页**.

   ![初始JSON请求 — SPA根](assets/navigation-routing/initial-json-request.png)

3. 打开开发人员工具后，导航到 **第3页**:

   ![第3页导航](assets/navigation-routing/page-three-navigation.png)

   请注意，新的XHR请求已发布到： `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理器了解 **第3页** JSON内容不可用，并会自动触发其他XHR请求。

4. 继续使用各种导航链接导航SPA。 请注意，未发出任何其他XHR请求，并且未发生完整的页面刷新。 这样，最终用户就可以快速获得SPA，并减少不必要的返回AEM请求。

   ![已实施导航](assets/navigation-routing/final-navigation-implemented.gif)

5. 通过直接导航到以下位置来体验深层链接： [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). 请注意，浏览器的“返回”按钮可继续工作。

## 恭喜！ {#congratulations}

恭喜，您了解了如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面来支持这些视图。 动态导航已通过Angular路由实施，并添加到 `Header` 组件。

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或通过切换到分支在本地检出代码 `Angular/navigation-routing-solution`.

### 后续步骤 {#next-steps}

[创建自定义组件](custom-component.md)  — 了解如何创建要与AEM SPA编辑器一起使用的自定义组件。 了解如何开发创作对话框和Sling模型以扩展JSON模型以填充自定义组件。
