---
title: 添加导航和路由 | AEM SPA编辑器和Angular入门
description: 了解如何使用AEM页面和SPA Editor SDK支持SPA中的多个视图。 动态导航是使用Angular路由实现的，并添加到现有的Header组件中。
sub-product: 站点
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2725'
ht-degree: 1%

---


# 添加导航和路由{#navigation-routing}

了解如何使用AEM页面和SPA Editor SDK支持SPA中的多个视图。 动态导航是使用Angular路由实现的，并添加到现有的Header组件中。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
2. 了解如何使用[Angular路由](https://angular.io/guide/router)在SPA的不同视图之间导航。
3. 实施由AEM页面层次结构驱动的动态导航。

## 您将构建的

本章将一个导航菜单添加到现有`Header`组件。 导航菜单由AEM页面层次结构驱动，并使用[导航核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![已实现导航](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

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

   如果使用[AEM 6.x](overview.md#compatibility)添加`classic`用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)安装完成的包。 将在WKND SPA上重新使用由[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的图像。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安装该包。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)上视图完成的代码，或通过切换到分支`Angular/navigation-routing-solution`在本地签出代码。

## Inspect HeaderComponent更新{#inspect-header}

在前几章中，将`HeaderComponent`组件添加为通过`app.component.html`包含的纯Angular组件。 在本章中，`HeaderComponent`组件将从应用程序中删除，并将通过[模板编辑器](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)添加。 这允许用户从AEM中配置`HeaderComponent`的导航菜单。

>[!NOTE]
>
> 已对代码库进行了多次CSS和JavaScript更新，以开始本章。 要专注于核心概念，不讨论代码更改的&#x200B;**所有**。 您可以在[此处](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start)视图完整更改。

1. 在您选择的IDE中，为本章打开SPA入门项目。
2. 在`ui.frontend`模块下，检查文件`header.component.ts`:`ui.frontend/src/app/components/header/header.component.ts`。

   已进行了多次更新，包括添加`HeaderEditConfig`和`MapTo`以使组件能够映射到AEM组件`wknd-spa-angular/components/header`。

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

   请注意`items`的`@Input()`注释。 `items` 将包含从AEM传入的导航对象数组。

3. 在`ui.apps`模块中，检查AEM `Header`组件的组件定义：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header`组件将通过`sling:resourceSuperType`属性继承[导航核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)的所有功能。

## 将HeaderComponent添加到SPA模板{#add-header-template}

1. 打开浏览器并登录AEM,[http://localhost:4502/](http://localhost:4502/)。 应已部署起始代码库。
2. 导航到&#x200B;**[!UICONTROL SPA页面模板]**:[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)。
3. 选择最外部的&#x200B;**[!UICONTROL 根布局容器]**，然后单击其&#x200B;**[!UICONTROL 策略]**&#x200B;图标。 请务必&#x200B;**不要**&#x200B;选择&#x200B;**[!UICONTROL 布局容器]**&#x200B;未锁定进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

4. 复制当前策略并创建名为&#x200B;**[!UICONTROL SPA Structure]**&#x200B;的新策略：

   ![SPA结构策略](assets/map-components/spa-policy-update.png)

   在&#x200B;**[!UICONTROL 允许的组件]** > **[!UICONTROL 常规]**&#x200B;下，选择&#x200B;**[!UICONTROL 布局容器]**&#x200B;组件。

   在&#x200B;**[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR- STRUCTURE]**&#x200B;下，选择&#x200B;**[!UICONTROL 标题]**&#x200B;组件：

   ![选择标题组件](assets/map-components/select-header-component.png)

   在&#x200B;**[!UICONTROL 允许的组件]** > **[!UICONTROL WKND SPAANGULAR- Content]** >下，选择&#x200B;**[!UICONTROL Image]**&#x200B;和&#x200B;**[!UICONTROL Text]**&#x200B;组件。 您应选择4个总组件。

   单击&#x200B;**[!UICONTROL 完成]**&#x200B;以保存更改。

5. **刷新页面。**&#x200B;在未锁定的&#x200B;**[!UICONTROL 布局容器]**&#x200B;上添加&#x200B;**[!UICONTROL Header]**&#x200B;组件：

   ![将标题组件添加到模板](./assets/navigation-routing/add-header-component.gif)

6. 选择&#x200B;**[!UICONTROL Header]**&#x200B;组件，然后单击其&#x200B;**Policy**&#x200B;图标以编辑策略。

   ![单击标题策略](assets/navigation-routing/header-policy-icon.png)

7. 使用&#x200B;******&quot;WKND SPA Header&quot;**&#x200B;的策略标题创建新策略。

   在&#x200B;**[!UICONTROL 属性]**&#x200B;下：

   * 将&#x200B;**[!UICONTROL 导航根]**&#x200B;设置为`/content/wknd-spa-angular/us/en`。
   * 将&#x200B;**[!UICONTROL 排除根级别]**&#x200B;设置为&#x200B;**1**。
   * 取消选中&#x200B;**[!UICONTROL 收集所有子页面]**。
   * 将&#x200B;**[!UICONTROL 导航结构深度]**&#x200B;设置为&#x200B;**3**。

   ![配置标头策略](assets/navigation-routing/header-policy.png)

   这将收集位于`/content/wknd-spa-angular/us/en`下方的导航2级。

8. 在保存更改后，您应会看到填充的`Header`作为模板的一部分：

   ![已填充的标题组件](assets/navigation-routing/populated-header.png)

## 创建子页面

接下来，在AEM中创建其他页面，作为SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到&#x200B;**站点**&#x200B;控制台：[http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home)。 选择&#x200B;**WKND SPAAngular主页**&#x200B;并单击&#x200B;**[!UICONTROL 创建]** > **[!UICONTROL 页面]**:

   ![创建新页面](assets/navigation-routing/create-new-page.png)

2. 在&#x200B;**[!UICONTROL 模板]**&#x200B;下，选择&#x200B;**[!UICONTROL SPA页面]**。 在&#x200B;**[!UICONTROL 属性]**&#x200B;下，输入&#x200B;**&quot;Page 1&quot;**&#x200B;作为&#x200B;**[!UICONTROL 标题]**&#x200B;和&#x200B;**&quot;page-1&quot;**&#x200B;的名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击&#x200B;**[!UICONTROL 创建]**，在对话框弹出窗口中，单击&#x200B;**[!UICONTROL 打开]**&#x200B;以在AEM SPA编辑器中打开页面。

3. 向主&#x200B;**[!UICONTROL 布局容器]**&#x200B;添加新的&#x200B;**[!UICONTROL Text]**&#x200B;组件。 编辑组件并输入文本：**&quot;第1页&quot;**&#x200B;使用RTE和&#x200B;**H1**&#x200B;元素（您必须进入全屏模式才能更改段落元素）

   ![示例内容第1页](assets/navigation-routing/page-1-sample-content.png)

   随意添加其他内容，如图像。

4. 返回AEM Sites控制台并重复上述步骤，创建名为&#x200B;**&quot;Page 2&quot;**&#x200B;的第二页，作为&#x200B;**Page 1**&#x200B;的同级页。 将内容添加到&#x200B;**第2**&#x200B;页，以便轻松识别。
5. 最后，创建第三页，**&quot;第3页&quot;**，但作为&#x200B;**第2页**&#x200B;的&#x200B;**子**。 完成后，站点层次结构应如下所示：

   ![示例网站层次结构](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 在新选项卡中，打开AEM提供的JSON模型API:[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 首次加载SPA时会请求此JSON内容。 外部结构如下所示：

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

   在`:children`下，您应当看到每个已创建页面对应的条目。 所有页面的内容都在此初始JSON请求中。 一旦实现导航路由，将快速加载SPA的后续视图，因为内容已在客户端可用。

   在初始JSON请求中加载SPA内容的&#x200B;**ALL**&#x200B;并不明智，因为这会降低初始页面加载速度。 接下来，我们将查看如何收集页面的层次结构深度。

7. 导航到&#x200B;**SPA Root**&#x200B;模板，网址为：[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html)。

   单击&#x200B;**[!UICONTROL 页面属性菜单]** > **[!UICONTROL 页面策略]**:

   ![打开SPA Root的页面策略](assets/navigation-routing/open-page-policy.png)

8. **SPA Root**&#x200B;模板中额外有一个&#x200B;**[!UICONTROL “分层结构]**”选项卡，用于控制收集的JSON内容。 **[!UICONTROL 结构深度]**&#x200B;确定站点层次中收集&#x200B;**根**&#x200B;下子页面的深度。 您还可以使用&#x200B;**[!UICONTROL 结构模式]**&#x200B;字段根据常规表达式筛选其他页面。

   将&#x200B;**[!UICONTROL 结构深度]**&#x200B;更新为&#x200B;**&quot;2&quot;**:

   ![更新结构深度](assets/navigation-routing/update-structure-depth.png)

   单击&#x200B;**[!UICONTROL 完成]**&#x200B;以保存对策略所做的更改。

9. 重新打开JSON模型[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

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

   请注意，**第3**&#x200B;页路径已被删除：`/content/wknd-spa-angular/us/en/home/page-2/page-3`。

   稍后，我们将观察AEM SPA Editor SDK如何动态加载其他内容。

## 实现导航

然后，使用新`NavigationComponent`实现导航菜单。 我们可以直接在`header.component.html`中添加代码，但最好避免使用大组件。 相反，应实现一个`NavigationComponent`，以后可能会重新使用它。

1. 查看位于[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)的AEM `Header`组件公开的JSON:

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

   AEM页面的分层性质在JSON中建模，可用于填充导航菜单。 记住，`Header`组件继承了[导航核心组件](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)的所有功能，通过JSON公开的内容将自动映射到Angular`@Input`注释。

2. 打开新的终端窗口，并导航到SPA项目的`ui.frontend`文件夹。 使用Angular CLI工具新建`NavigationComponent`:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 然后使用新创建的`components/navigation`目录中的Angular CLI创建名为`NavigationLink`的类：

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 返回到您选择的IDE，在`/src/app/components/navigation/navigation-link.ts`的`navigation-link.ts`处打开文件。

   ![打开navigation-link.ts文件](assets/navigation-routing/ide-navigation-link-file.png)

5. 使用以下内容填充`navigation-link.ts`:

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

   这是一个表示单个导航链接的简单类。 在类构造函数中，我们希望`data`是从AEM传入的JSON对象。 此类将同时用于`NavigationComponent`和`HeaderComponent`中以轻松填充导航结构。

   未执行任何数据转换，此类主要创建为强烈键入JSON模型。 请注意，`this.children`的类型为`NavigationLink[]`，并且构造函数递归地为`children`数组中的每个项创建新的`NavigationLink`对象。 回想一下，`Header`的JSON模型是分层的。

6. 打开文件`navigation-link.spec.ts`。 这是`NavigationLink`类的测试文件。 请使用以下内容更新它：

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

   请注意，`const data`遵循之前为单个链接检查的相同JSON模型。 这远非一个强健的单元测试，但是，测试`NavigationLink`的构造函数应该足够。

7. 打开文件`navigation.component.ts`。 请使用以下内容更新它：

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

   `NavigationComponent` 需要 `object[]` 一 `items` 个名为的JSON模型。此类公开一个方法`get navigationLinks()`，它返回一个`NavigationLink`对象数组。

8. 打开文件`navigation.component.html`并使用以下命令更新它：

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   这将生成初始`<ul>`，并从`navigation.component.ts`调用`get navigationLinks()`方法。 `<ng-container>`用于调用名为`recursiveListTmpl`的模板，并将`navigationLinks`作为名为`links`的变量传递给它。

   添加下一个`recursiveListTmpl`:

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

   此处实现了导航链接的其余渲染。 请注意，变量`link`的类型为`NavigationLink`，该类创建的所有方法/属性均可用。 [`[routerLink]`](https://angular.io/api/router/RouterLink) is used而不是normal `href` 属性。这样，我们无需刷新整页即可链接到应用程序中的特定路由。

   如果当前`link`具有非空`children`数组，则通过创建另一个`<ul>`实现导航的递归部分。

9. 更新`navigation.component.spec.ts`以添加对`RouterTestingModule`的支持：

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

   需要添加`RouterTestingModule`，因为组件使用`[routerLink]`。

10. 更新`navigation.component.scss`以向`NavigationComponent`添加一些基本样式：

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

## 更新标题组件

现在`NavigationComponent`已实现，必须更新`HeaderComponent`以引用它。

1. 打开终端并导航到SPA项目中的`ui.frontend`文件夹。 开始&#x200B;**webpack dev server**:

   ```shell
   $ npm start
   ```

2. 打开浏览器选项卡并导航到[http://localhost:4200/](http://localhost:4200/)。

   应将&#x200B;**webpack dev server**&#x200B;配置为从AEM的本地实例代理JSON模型(`ui.frontend/proxy.conf.json`)。 这将允许我们直接针对在教程前面部分的AEM中创建的内容进行编码。

   ![菜单切换工作](./assets/navigation-routing/nav-toggle-static.gif)

   `HeaderComponent`当前已实现菜单切换功能。 然后，添加导航组件。

3. 返回到您选择的IDE，然后在`ui.frontend/src/app/components/header/header.component.ts`打开文件`header.component.ts`。
4. 更新`setHomePage()`方法以删除硬编码字符串并使用AEM组件传入的动态prop:

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

   将根据从AEM传入的导航JSON模型的根`items[0]`创建`NavigationLink`的新实例。 `this.route.snapshot.data.path` 返回当前Angular路由的路径。此值用于确定当前路由是否为&#x200B;**主页**。 `this.homePageUrl` 用于填充徽标上的锚点 **链接**。

5. 打开`header.component.html`并将导航的静态占位符替换为对新创建的`NavigationComponent`的引用：

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` attribute将 `@Input() items` 从 `HeaderComponent` 传 `NavigationComponent` 递到它构建导航的位置。

6. 打开`header.component.spec.ts`并添加`NavigationComponent`的声明：

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

   由于`NavigationComponent`现在用作`HeaderComponent`的一部分，因此需要将其声明为测试台的一部分。

7. 保存对任何打开的文件所做的更改并返回至&#x200B;**Webpack dev server**:[http://localhost:4200/](http://localhost:4200/)

   ![已完成标题导航](assets/navigation-routing/completed-header.png)

   单击菜单切换打开导航，您应会看到已填充的导航链接。 您应该能够导航到SPA的不同视图。

## 了解SPA路由

现在已实施导航，请检查AEM中的路由。

1. 在IDE中，打开位于`ui.frontend/src/app`的文件`app-routing.module.ts`。

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

   `routes: Routes = [];`数组定义指向Angular组件映射的路由或导航路径。

   `AemPageMatcher` 是自定义Angular路 [由器UrlMatcher](https://angular.io/api/router/UrlMatcher)，它匹配AEM中属于此Angular应用程序的任何页面的“外观”。

   `PageComponent` 是在AEM中表示页面的Angular组件，匹配的路由将调用。将进一步检查`PageComponent`。

   `AemPageDataResolver`由AEM SPA Editor JS SDK提供的是自定义 [Angular路由](https://angular.io/api/router/Resolve) 器解析器，用于将路由URL(即AEM中包含.html扩展的路径)转换为AEM中的资源路径（即页面路径减去扩展名）。

   例如，`AemPageDataResolver`将路由的URL `content/wknd-spa-angular/us/en/home.html`转换为路径`/content/wknd-spa-angular/us/en/home`。 这用于根据JSON模型API中的路径解析页面内容。

   `AemPageRouteReuseStrategy`由AEM SPA Editor JS SDK提供的自定义RouteReuseStrategy可 [](https://angular.io/api/router/RouteReuseStrategy) 防止跨路由 `PageComponent` 重用。否则，页面“A”中的内容在导航到页面“B”时可能会显示。

2. 在`ui.frontend/src/app/components/page/`打开文件`page.component.ts`。

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

   需要`PageComponent`才能处理从AEM检索到的JSON，并用作呈现路由的Angular组件。

   `ActivatedRoute`，由Angular路由器模块提供，包含状态，指示应将AEM页面的JSON内容加载到此Angular页面组件实例中。

   `ModelManagerService`，根据路由获取JSON数据，并将数据映射到类变 `path`量 `items`, `itemsOrder`。然后，这些组件将传递给[AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. 在`ui.frontend/src/app/components/page/`打开文件`page.component.html`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 包括 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)。变量`path`、`items`和`itemsOrder`将传递给`AEMPageComponent`。 然后，通过SPA Editor JavaScript SDK提供的`AemPageComponent`将对该数据进行迭代，并根据[映射组件教程](./map-components.md)中所示的JSON数据动态实例化Angular组件。

   `PageComponent`实际上只是`AEMPageComponent`的代理，它是`AEMPageComponent`执行大部分重力提升以将JSON模型正确映射到Angular组件的代理。

## Inspect SPA 路由 AEM

1. 打开终端，如果启动，则停止&#x200B;**webpack dev server**。 导航到项目的根，然后使用您的Maven技能将项目部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular项目启用了一些非常严格的linting规则。 如果Maven生成失败，请检查错误并查找列出的文件中找到的&#x200B;**Lint错误。**. 修复linter发现的所有问题并重新运行Maven命令。

2. 导航到AEM中的SPA主页：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html)并打开浏览器的开发人员工具。 下面的屏幕截图是从Google Chrome浏览器中捕获的。

   刷新页面，您会看到对`/content/wknd-spa-angular/us/en.model.json`的XHR请求，该请求是SPA根。 请注意，根据本教程前面部分对SPA根模板进行的层次结构深度配置，只包含三个子页面。 这不包括&#x200B;**第3**&#x200B;页。

   ![初始JSON请求 — SPA根](assets/navigation-routing/initial-json-request.png)

3. 打开开发人员工具后，导航到&#x200B;**第3**&#x200B;页：

   ![第3页导航](assets/navigation-routing/page-three-navigation.png)

   请注意，新的XHR请求旨在：`/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager了解&#x200B;**第3**&#x200B;页的JSON内容不可用，并会自动触发其他XHR请求。

4. 使用各种导航链接继续导航SPA。 请注意，未发出任何其他XHR请求，且未发生完整页面刷新。 这使得SPA对最终用户而言更快，并减少了返回AEM的不必要请求。

   ![已实现导航](assets/navigation-routing/final-navigation-implemented.gif)

5. 通过直接导航到：[http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html)。 请注意，浏览器的“后退”按钮仍然有效。

## 恭喜！{#congratulations}

祝贺您，您学习了如何通过SPA Editor SDK映射到AEM页面来支持SPA中的多个视图。 动态导航已使用Angular路由实现并添加到`Header`组件。

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)上视图完成的代码，或通过切换到分支`Angular/navigation-routing-solution`在本地签出代码。

### 后续步骤{#next-steps}

[创建自定义组件](custom-component.md)  — 了解如何创建要与AEM SPA Editor一起使用的自定义组件。了解如何开发创作对话框和Sling Models以扩展JSON模型以填充自定义组件。
