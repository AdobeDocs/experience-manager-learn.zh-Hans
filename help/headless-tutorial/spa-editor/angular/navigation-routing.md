---
title: 添加导航和路由 | AEM SPA编辑器和Angular快速入门
description: 了解如何使用SPA页面和AEM编辑器SDK支持SPA中的多个视图。 动态导航是使用Angular路由实现的，并且已添加到现有的标题组件中。
feature: SPA Editor
version: Cloud Service
jira: KT-5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
duration: 854
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2531'
ht-degree: 0%

---

# 添加导航和路由 {#navigation-routing}

了解如何使用SPA页面和AEM编辑器SDK支持SPA中的多个视图。 动态导航是使用Angular路由实现的，并且已添加到现有的标题组件中。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
2. 了解如何使用 [angular路由](https://angular.io/guide/router) 在SPA的不同视图之间导航。
3. 实施由AEM页面层次结构驱动的动态导航。

## 您将构建的内容

本章将导航菜单添加到现有的 `Header` 组件。 导航菜单由AEM页面层次结构驱动，并使用由提供的JSON模型。 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![已实施导航](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment).

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

   如果使用 [AEM 6.x](overview.md#compatibility) 添加 `classic` 个人资料：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统安装完成的包 [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest). 图片提供方： [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 在WKND SPA上重复使用。 可使用以下方式安装软件包 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或通过切换到分支在本地签出代码 `Angular/navigation-routing-solution`.

## Inspect标题组件更新 {#inspect-header}

在前几章中， `HeaderComponent` 通过添加组件作为纯Angular组件 `app.component.html`. 在本章中， `HeaderComponent` 组件将从应用程序中删除，并通过以下方式添加： [模板编辑器](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). 这允许用户配置 `HeaderComponent` 从AEM中。

>[!NOTE]
>
> 已对代码库进行了多次CSS和JavaScript更新，以开始阅读本章。 关注核心概念，而非 **所有** 对代码的变化进行了讨论。 您可以查看全部更改 [此处](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. 在您选择的IDE中，打开本章的SPA入门项目。
2. 在 `ui.frontend` 模块检查文件 `header.component.ts` 在： `ui.frontend/src/app/components/header/header.component.ts`.

   进行了一些更新，包括添加了 `HeaderEditConfig` 和 `MapTo` 启用该组件以映射到AEM组件 `wknd-spa-angular/components/header`.

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

3. 在 `ui.apps` 模块检查AEM的组件定义 `Header` 组件： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` 组件将继承的所有功能 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) 通过 `sling:resourceSuperType` 属性。

## 将HeaderComponent添加到SPA模板 {#add-header-template}

1. 打开浏览器并登录到AEM， [http://localhost:4502/](http://localhost:4502/). 应该已经部署了起始代码库。
2. 导航至 **[!UICONTROL SPA页面模板]**： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 选择最外层的 **[!UICONTROL 根布局容器]** 并单击其 **[!UICONTROL 策略]** 图标。 小心点 **非** 以选择 **[!UICONTROL 布局容器]** 已解锁进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

4. 复制当前策略并创建一个名为的新策略 **[!UICONTROL SPA结构]**：

   ![SPA结构策略](assets/map-components/spa-policy-update.png)

   下 **[!UICONTROL 允许的组件]** > **[!UICONTROL 常规]** >选择 **[!UICONTROL 布局容器]** 组件。

   下 **[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR — 结构]** >选择 **[!UICONTROL 页眉]** 组件：

   ![选择标题组件](assets/map-components/select-header-component.png)

   下 **[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR — 内容]** >选择 **[!UICONTROL 图像]** 和 **[!UICONTROL 文本]** 组件。 您总共应选择4个组件。

   单击&#x200B;**[!UICONTROL 完成]**&#x200B;以保存更改。

5. **刷新** 页面。 添加 **[!UICONTROL 页眉]** 组件位于已解锁状态上方 **[!UICONTROL 布局容器]**：

   ![将标题组件添加到模板](./assets/navigation-routing/add-header-component.gif)

6. 选择 **[!UICONTROL 页眉]** 组件并单击其 **策略** 图标以编辑策略。

   ![单击标题策略](assets/navigation-routing/header-policy-icon.png)

7. 使用创建新策略 **[!UICONTROL 策略标题]** 之 **&quot;WKND SPA标头&quot;**.

   在 **[!UICONTROL 属性]**：

   * 设置 **[!UICONTROL 导航根目录]** 到 `/content/wknd-spa-angular/us/en`.
   * 设置 **[!UICONTROL 排除根级别]** 到 **1**.
   * 取消选中 **[!UICONTROL 收集所有子页面]**.
   * 设置 **[!UICONTROL 导航结构深度]** 到 **3**.

   ![配置标头策略](assets/navigation-routing/header-policy.png)

   这将收集下方2个级别的导航 `/content/wknd-spa-angular/us/en`.

8. 保存更改后，您应会看到已填充 `Header` 作为模板的一部分：

   ![填充的标题组件](assets/navigation-routing/populated-header.png)

## 创建子页面

接下来，在AEM中创建其他页面，这些页面将用作SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航至 **站点** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). 选择 **WKND SPAAngular主页** 并单击 **[!UICONTROL 创建]** > **[!UICONTROL 页面]**：

   ![创建新页面](assets/navigation-routing/create-new-page.png)

2. 下 **[!UICONTROL 模板]** 选择 **[!UICONTROL SPA页面]**. 下 **[!UICONTROL 属性]** 进入 **&quot;第1页&quot;** 对于 **[!UICONTROL 标题]** 和 **&quot;page-1&quot;** 作为名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击 **[!UICONTROL 创建]** 在对话框弹出窗口中，单击 **[!UICONTROL 打开]** 以在AEM SPA编辑器中打开该页面。

3. 添加新 **[!UICONTROL 文本]** 组件到主 **[!UICONTROL 布局容器]**. 编辑组件并输入文本： **&quot;第1页&quot;** 使用RTE和 **H1** 元素（您必须进入全屏模式才能更改段落元素）

   ![示例内容页面1](assets/navigation-routing/page-1-sample-content.png)

   您可以随意添加其他内容，如图像。

4. 返回到AEM Sites控制台并重复上述步骤，创建名为的第二个页面 **“第2页”** 作为兄弟姐妹 **第1页**. 将内容添加到 **第2页** 以便于识别。
5. 最后，创建第三页， **“第3页”** 但作为 **子项** 之 **第2页**. 完成之后，站点层级应如下所示：

   ![示例站点层次结构](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新选项卡中，打开由AEM提供的JSON模型API： [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 首次加载SPA时会请求此JSON内容。 外部结构如下所示：

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

   下 `:children` 您应该会看到所创建的每个页面的条目。 所有页面的内容都包含在此初始JSON请求中。 一旦实现了导航路由，由于内容在客户端已经可用，SPA的后续视图将快速加载。

   装载是不明智的 **所有** SPA内容的ID，因为这会减慢初始页面加载的速度。 接下来，我们来看看如何收集页面的层级深度。

7. 导航至 **SPA根目录** 模板位于： [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   单击 **[!UICONTROL 页面属性菜单]** > **[!UICONTROL 页面策略]**：

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

8. 此 **SPA根目录** 模板有一个额外的 **[!UICONTROL 层次结构]** 选项卡来控制收集的JSON内容。 此 **[!UICONTROL 结构深度]** 确定网站层次结构中收集子页面的深度 **根**. 您也可以使用 **[!UICONTROL 结构模式]** 用于根据正则表达式筛选掉其他页面的字段。

   更新 **[!UICONTROL 结构深度]** 到 **“2”**：

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

   请注意 **第3页** 路径已删除： `/content/wknd-spa-angular/us/en/home/page-2/page-3` 来自初始JSON模型。

   稍后，我们将观察AEM SPA Editor SDK如何动态加载其他内容。

## 实施导航

接下来，使用新的菜单实施导航菜单 `NavigationComponent`. 我们可以直接在中添加代码 `header.component.html` 但更好的做法是避免使用大型部件。 相反，请实施 `NavigationComponent` 以后可能会重复使用。

1. 查看AEM公开的JSON `Header` 组件位于 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)：

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

   AEM页面的层次特性在JSON中建模，可用于填充导航菜单。 请记住 `Header` 组件继承了的所有功能 [导航核心组件](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) 并且通过JSON公开的内容会自动映射到Angular `@Input` 注释。

2. 打开新的终端窗口并导航到 `ui.frontend` SPA项目的文件夹。 新建 `NavigationComponent` 使用AngularCLI工具：

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 接下来，创建一个名为的类 `NavigationLink` 在新创建的中使用AngularCLI `components/navigation` 目录：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回您选择的IDE并在以下位置打开文件： `navigation-link.ts` 在 `/src/app/components/navigation/navigation-link.ts`.

   ![打开navigation-link.ts文件](assets/navigation-routing/ide-navigation-link-file.png)

5. 填充 `navigation-link.ts` ，如下所示：

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

   这是一个简单的类，用于表示单个导航链接。 在类构造函数中 `data` 作为从AEM传入的JSON对象。 此类同时用于 `NavigationComponent` 和 `HeaderComponent` 以轻松填充导航结构。

   不执行数据转换，此类主要是为强键入JSON模型而创建的。 请注意 `this.children` 键入为 `NavigationLink[]` 并且构造函数递归地创建 `NavigationLink` 中每个项目的对象 `children` 数组。 记住JSON模型 `Header` 是分层的。

6. 打开文件 `navigation-link.spec.ts`. 这是的测试文件 `NavigationLink` 类。 使用以下内容更新它：

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

   请注意 `const data` 遵循之前检查的同一个JSON模型以获取单个链接。 这远远不是强大的单元测试，但是它应该足以测试构造函数 `NavigationLink`.

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

   `NavigationComponent` 应为 `object[]` 已命名 `items` 这是AEM中的JSON模型。 此类公开单个方法 `get navigationLinks()` 返回数组 `NavigationLink` 对象。

8. 打开文件 `navigation.component.html` 并使用以下内容更新它：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   这将生成一个初始 `<ul>` 并调用 `get navigationLinks()` 方法自 `navigation.component.ts`. An `<ng-container>` 用于调用名为的模板 `recursiveListTmpl` 然后传递给 `navigationLinks` 作为变量，名为 `links`.

   添加 `recursiveListTmpl` 下一步：

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

   此时将实施导航链接的其余渲染。 请注意，变量 `link` 属于类型 `NavigationLink` 并且该类创建的所有方法/属性均可用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) 而不是正常使用 `href` 属性。 这样，我们便可链接到应用程序中的特定路由，而无需刷新全页。

   导航的递归部分也通过创建另一个 `<ul>` 如果当前 `link` 具有非空 `children` 数组。

9. 更新 `navigation.component.spec.ts` 添加支持 `RouterTestingModule`：

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

10. 更新 `navigation.component.scss` 将一些基本样式添加到 `NavigationComponent`：

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

现在， `NavigationComponent` 已实施， `HeaderComponent` 必须更新以引用它。

1. 打开终端并导航到 `ui.frontend` 文件夹(在SPA项目中)。 启动 **webpack开发服务器**：

   ```shell
   $ npm start
   ```

2. 打开浏览器选项卡并导航至 [http://localhost:4200/](http://localhost:4200/).

   此 **webpack开发服务器** 应配置为从AEM的本地实例代理JSON模型(`ui.frontend/proxy.conf.json`)。 这样，我们就可以直接针对本教程前面介绍的在AEM中创建的内容进行编码。

   ![菜单切换工作](./assets/navigation-routing/nav-toggle-static.gif)

   此 `HeaderComponent` 当前已实施菜单切换功能。 接下来，添加导航组件。

3. 返回到您选择的IDE并打开文件 `header.component.ts` 在 `ui.frontend/src/app/components/header/header.component.ts`.
4. 更新 `setHomePage()` 方法以删除硬编码字符串并使用AEM组件传入的动态属性：

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

   的新实例 `NavigationLink` 创建于 `items[0]`，从AEM传入的导航JSON模型的根。 `this.route.snapshot.data.path` 返回当前Angular路由的路径。 此值用于确定当前路由是否为 **主页**. `this.homePageUrl` 用于填充上的锚点链接 **徽标**.

5. 打开 `header.component.html` 并将导航的静态占位符替换为对新创建的引用 `NavigationComponent`：

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 属性传递 `@Input() items` 从 `HeaderComponent` 到 `NavigationComponent` 将构建导航的位置。

6. 打开 `header.component.spec.ts` 并添加声明 `NavigationComponent`：

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

   由于 `NavigationComponent` 现在用作 `HeaderComponent` 它需要被宣告为测试平台的一部分。

7. 保存对任何打开文件的更改并返回到 **webpack开发服务器**： [http://localhost:4200/](http://localhost:4200/)

   ![完成的标题导航](assets/navigation-routing/completed-header.png)

   通过单击菜单切换打开导航，您应该会看到填充的导航链接。 您应该能够导航到SPA的不同视图。

## 了解SPA路由

现在导航已经实现，请在AEM中检查路由。

1. 在IDE中打开文件 `app-routing.module.ts` 在 `ui.frontend/src/app`.

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

   此 `routes: Routes = [];` 数组定义Angular组件映射的路由或导航路径。

   `AemPageMatcher` 是自定义Angular路由器 [UrlMatcher](https://angular.io/api/router/UrlMatcher)，用于匹配AEM中属于此Angular应用程序一部分的“看起来”页面的任何内容。

   `PageComponent` 是AEM中表示页面的Angular组件，用于呈现匹配的路由。 此 `PageComponent` 将在本教程的后面部分进行回顾。

   `AemPageDataResolver`由AEM SPA编辑器JS SDK提供的，是一个自定义 [angular路由器解析程序](https://angular.io/api/router/Resolve) 用于将路由URL(AEM中包含.html扩展名的路径)转换为AEM中的资源路径（页面路径减去扩展名）。

   例如， `AemPageDataResolver` 将路由的URL转换为 `content/wknd-spa-angular/us/en/home.html` 到路径 `/content/wknd-spa-angular/us/en/home`. 此标头用于根据JSON模型API中的路径解析页面内容。

   `AemPageRouteReuseStrategy`由AEM SPA编辑器JS SDK提供的，是一个自定义 [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) 防止重复使用 `PageComponent` 跨越路由。 否则，在导航到页面“B”时，可能会显示页面“A”中的内容。

2. 打开文件 `page.component.ts` 在 `ui.frontend/src/app/components/page/`.

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

   此 `PageComponent` 是处理从AEM检索到的JSON所必需的，并且将用作Angular组件以呈现路由。

   `ActivatedRoute`，由Angular路由器模块提供，包含指示应将哪个AEM页面的JSON内容加载到此Angular页面组件实例中的状态。

   `ModelManagerService`，根据路由获取JSON数据并将该数据映射到类变量 `path`， `items`， `itemsOrder`. 这些代码随后将传递到 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 打开文件 `page.component.html` 在 `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包括 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). 变量 `path`， `items`、和 `itemsOrder` 传递到 `AEMPageComponent`. 此 `AemPageComponent`，然后通过SPA编辑器JavaScript SDK提供，将对此数据进行迭代，并根据JSON数据动态实例化Angular组件，如 [映射组件教程](./map-components.md).

   此 `PageComponent` 只是个代表 `AEMPageComponent` 而是 `AEMPageComponent` 这完成了大部分繁重的工作以将JSON模型正确映射到Angular组件。

## 在AEM中Inspect SPA路由

1. 打开终端并停止 **webpack开发服务器** （如果已启动）。 导航到项目的根，然后使用您的Maven技能将该项目部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular项目启用了一些非常严格的筛选规则。 如果Maven构建失败，请检查错误并查找 **在列出的文件中发现链接错误。**. 修复过滤器发现的任何问题并重新运行Maven命令。

2. 导航到AEM中的SPA主页： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) 并打开浏览器的开发人员工具。 以下屏幕截图是从Google Chrome浏览器中捕获的。

   刷新该页，您应该会看到一个XHR请求 `/content/wknd-spa-angular/us/en.model.json`，即SPA根。 请注意，根据教程前面部分对SPA根模板的层级深度配置，只包含三个子页面。 这不包括 **第3页**.

   ![初始JSON请求 — SPA根目录](assets/navigation-routing/initial-json-request.png)

3. 在开发人员工具打开时，导航到 **第3页**：

   ![第3页导航](assets/navigation-routing/page-three-navigation.png)

   请注意，已发出新的XHR请求以： `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理器了解 **第3页** JSON内容不可用，将自动触发其他XHR请求。

4. 继续使用各种导航链接在SPA中导航。 请注意，不会发出其他XHR请求，也不会进行全页刷新。 这使得最终用户能够快速使用SPA，并减少返回AEM的不必要请求。

   ![已实施导航](assets/navigation-routing/final-navigation-implemented.gif)

5. 通过直接导航到，尝试使用深层链接： [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). 请注意，浏览器的“后退”按钮仍可继续使用。

## 恭喜！ {#congratulations}

恭喜，您已了解如何通过使用SPA编辑器SDK映射到AEM页面来支持SPA中的多个视图。 已使用Angular路由实施动态导航，并将其添加到 `Header` 组件。

您始终可以在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) 或通过切换到分支在本地签出代码 `Angular/navigation-routing-solution`.

### 后续步骤 {#next-steps}

[创建自定义组件](custom-component.md)  — 了解如何创建要与AEM SPA编辑器一起使用的自定义组件。 了解如何开发创作对话框和Sling模型，以扩展JSON模型来填充自定义组件。
