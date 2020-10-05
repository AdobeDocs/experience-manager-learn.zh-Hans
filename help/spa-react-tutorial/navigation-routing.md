---
title: 添加导航和路由 | AEM SPA编辑器和React入门
description: 了解如何通过使用SPA编辑器SDK映射到AEM页面来支持SPA中的多个视图。 动态导航是使用React Router实现的，并添加到现有的Header组件中。
sub-product: 站点
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 1%

---


# 添加导航和路由 {#navigation-routing}

了解如何通过使用SPA编辑器SDK映射到AEM页面来支持SPA中的多个视图。 动态导航是使用React Router实现的，并添加到现有的Header组件中。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
1. 了解如何使 [用React Router](https://reacttraining.com/react-router/) 在SPA的不同视图之间导航。
1. 实现由AEM页面层次结构驱动的动态导航。

## 您将构建的内容

本章将向现有组件添加导航 `Header` 菜单。 导航菜单将由AEM页面层次结构驱动，并将利用导航核心组件提供 [的JSON模型](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)。

![实现导航](assets/navigation-routing/final-navigation-implemented.gif)

## 前提条件

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 为传统WKND参考站点安 [装完成的包](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKND参考站 [点提供的图像](https://github.com/adobe/aem-guides-wknd/releases/latest) ，将在WKND SPA上重新使用。 可以使用AEM Package Manager [安装包](http://localhost:4502/crx/packmgr/index.jsp)。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ，或通过切换到分支在本地签出代码 `React/navigation-routing-solution`。

## Inspect头更新 {#inspect-header}

在前几章中，将该组 `Header` 件添加为包含在中的纯React组件 `App.js`。 在本章中，该组 `Header` 件已被删除，并将通过模板编辑 [器添加](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)。 这将允许用户从AEM中配置导航 `Header` 菜单。

>[!NOTE]
>
> 已对代码库进行了多次CSS和JavaScript更新，以开始本章。 为了集中讨论核心概念， **并不讨论** 所有代码更改。 您可以在此处视图完 [整更改](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)。

1. 在您选择的IDE中，打开本章的SPA入门项目。
1. 在模块 `ui.frontend` 下方，检查文 `Header.js` 件： `ui.frontend/src/components/Header/Header.js`.

   已进行了多次更新，包括添加 `HeaderEditConfig` 了 `MapTo` 一个和一个以使组件能够映射到AEM组件 `wknd-spa-react/components/header`。

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. 在模块 `ui.apps` 中，检查AEM组件的组件定 `Header` 义： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   AEM组 `Header` 件将通过属性继承导航核 [心组件的](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) 所有 `sling:resourceSuperType` 功能。

## 将标题添加到模板 {#add-header-template}

1. 打开浏览器并登录AEM, [http://localhost:4502/](http://localhost:4502/)。 应已部署起始代码库。
1. 导航到 **SPA页面模板**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 选择最外侧的“根 **布局”容器** ，然后单 **击“策略** ”图标。 切勿选 **择布** 局容器 **，取消** 锁定，进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

1. 创建名为SPA结构 **的新策略**:

   ![SPA结构策略](assets/navigation-routing/spa-policy-update.png)

   在“允 **许的组件** ” > “ **常规** ” >下，选 **择布局容器组件** 。

   在“允 **许的组件** ” > **WKND SPA REACT - STRUCTURE** >下选择 **Header组件** :

   ![选择标题组件](assets/navigation-routing/select-header-component.png)

   在“允 **许的组件** ” > **WKND SPA REACT —— 内容** >选择图 **像和** 文本 **** 组件。 您应选择4个总组件。

   Click **Done** to save the changes.

1. 刷新页面，并在未锁 **定的** “布局”容器上添 **加Header组件**:

   ![将标题组件添加到模板](./assets/navigation-routing/add-header-component.gif)

1. 选择标 **头组件** ，然后单击其 **策略图** 标以编辑策略。
1. 使用WKND SPA标头 **的策略****标题创建新策略**。

   在属 **性下**:

   * 将导航 **根设置** 为 `/content/wknd-spa-react/us/en`。
   * 将“排 **除根级别** ”设 **置为1**。
   * Uncheck **Collect all child pages**.
   * 将“导航 **结构深度** ”设 **置为3**。

   ![配置标头策略](assets/navigation-routing/header-policy.png)

   这将收集深处的导航2级 `/content/wknd-spa-react/us/en`。

1. 保存更改后，您应将填充的内 `Header` 容视为模板的一部分：

   ![已填充的标题组件](assets/navigation-routing/populated-header.png)

## 创建子页面

接下来，在AEM中创建其他页面，作为SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到站 **点** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 选择WKND **SPA React主页** ，然 **后单击** 创建 **>**&#x200B;页：

   ![创建新页面](assets/navigation-routing/create-new-page.png)

1. 在模 **板下** ，选 **择SPA页面**。 在 **属** 性 **下** , **为Title输入Page** **1,** 并输入Page-1的名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击 **创建** ，在对话框弹出窗口中，单 **击打** 开，以在AEM SPA编辑器中打开页面。

1. 向主布 **局容器** 添加新的 **文本组件**。 编辑组件并输入文本： **第1页** ，使用RTE和 **H1元素** （您必须进入全屏模式才能更改段落元素）

   ![示例内容第1页](assets/navigation-routing/page-1-sample-content.png)

   随意添加其他内容，如图像。

1. 返回AEM Sites控制台并重复上述步骤，创建名为Page **2的第二页** ，作为Page **1的兄弟页**。
1. 最后创建第三页， **第3页** ，但 **是第** 2页的 **子页**。 完成后，站点层次结构应如下所示：

   ![示例站点层次结构](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 在新选项卡中，打开AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 首次加载SPA时请求此JSON内容。 外部结构如下所示：

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {},
       "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
       }
   }
   ```

   在下 `:children` 面，您应当看到创建的每个页面的条目。 所有页面的内容都在此初始JSON请求中。 一旦实施了导航路由,SPA的后续视图将被快速加载，因为内容在客户端已经可用。

   在初始JSON请求中 **加载** SPA的所有内容并不明智，因为这会减慢初始页面加载。 接下来，我们来了解如何收集页面的层次结构深度。

1. 导航到SPA **根模板** ，网址为： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)。

   单击“页 **面属性”菜单** > **页面策略**:

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

1. SPA根 **模板中** ，还有一个额外的“ **分层结构** ”选项卡，用于控制收集的JSON内容。 结 **构深度** (Structure Depth)确定在站点层次结构中收集根下子页面的 **深度**。 您还可以使用“结 **构模式** ”字段根据常规表达式筛选其他页面。

   将“结构 **深度** ”更新 **为2**:

   ![更新结构深度](assets/navigation-routing/update-structure-depth.png)

   单击 **完成** ，以保存对策略所做的更改。

1. 重新打开JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {}
       }
   }
   ```

   请注意， **页面** 3路径已被删除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 从初始JSON模型开始。

   稍后，我们将观察AEM SPA Editor SDK如何动态加载其他内容。

## 实现导航

接下来，将导航菜单作为的一部分进行实 `Header`现。 我们可以直接在中添加代 `Header.js` 码，但最好的做法是避免使用大型组件。 相反，我们将实施一 `Navigation` 个SPA组件，以后可能会重新使用。

1. 查看AEM组件在http://localhost:4502/content/wknd-spa-react/us/en.model.json上公 `Header` 开的 [JSON](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   AEM页面的分层性质在JSON中建模，可用于填充导航菜单。 请记住， `Header` 该组件继承了导航核心组 [件的所有功能](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) ，通过JSON公开的内容将自动映射到React prop。

1. 打开新的终端窗口并导 `ui.frontend` 航到SPA项目的文件夹。 使用 **命令开始webpack** -dev-server `npm start`。

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. 打开新的浏览器选项卡并导航到 [http://localhost:3000/](http://localhost:3000/)。

   应 **将webpack-dev** -server配置为从AEM()的本地实例代理JSON模`ui.frontend/.env.development`型。 这将允许我们直接针对在上一个练习中在AEM中创建的内容进行编码。 确保在同一浏览会话中对您进行了AEM身份验证。

   ![菜单切换工作](./assets/navigation-routing/nav-toggle-static.gif)

   当前 `Header` 已实现菜单切换功能。 然后，实现导航菜单。

1. 返回您选择的IDE并打开 `Header.js` 位 `ui.frontend/src/components/Header/Header.js`置
1. 更新方 `homeLink()` 法以删除硬编码的字符串，并使用AEM组件传入的动态prop:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   上述代码将根据组件配置的根导航项填充url。 `homeLink()` 用于填充方法中的标 `logo()` 志，并用于确定是否应在中显示返回按钮 `backButton()`。

   将更改保存 `Header.js`到。

1. 在顶部添加一行，以 `Header.js` 在其他导入 `Navigation` 项下导入组件：

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. 下一步更新 `get navigation()` 方法以实例化 `Navigation` 组件：

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   如前所述，我们将在组件中实 `Header` 现大部分逻辑，而不是在组件中实 `Navigation` 现导航。  prop包括构 `Header` 建菜单所需的JSON结构，我们传递了所有prop。
1. 在打开文 `Navigation.js` 件 `ui.frontend/src/components/Navigation/Navigation.js`。
1. 实现方 `renderGroupNav(children)` 法：

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   此方法采用导航项的数组， `children`并创建未排序的列表。 然后，它迭代该数组并将该项传递 `renderNavItem`给该项，随后将实现。

1. 实施 `renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   此方法呈现列表项，其CSS类基于属性 `level` 和 `active`。 然后，该方法 `renderLink` 调用以创建锚点标记。 由于 `Navigation` 内容是分层的，因此使用递归策略来调 `renderGroupNav` 用当前项的子项。

1. 实现方 `renderLink` 法：

   在文件顶部为Link [组件](https://reacttraining.com/react-router/web/api/Link) （React路由器的一部分）添加一个导入方法，其他导入方式如下：

   ```js
   import {Link} from "react-router-dom";
   ```

   接下来完成该方法的 `renderLink` 实现：

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   请注意，Link组件使用的不是 `<a>`普通的 [锚点](https://reacttraining.com/react-router/web/api/Link) 标签。 这可确保不触发整个页面刷新，而是利用AEM SPA Editor JS SDK提供的React路由器。

1. 保存更改 `Navigation.js` 并返回 **到webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![已完成标题导航](assets/navigation-routing/completed-header.png)

   单击菜单切换打开导航，您应当看到已填充的导航链接。 您应该能够导航到SPA的不同视图。

## Inspect温泉路由

现在，已实施导航，请检查AEM中的路由。

1. 在IDE中，打开位 `index.js` 于的文 `ui.frontend/src/index.js`件。

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   请注意，该 `App` 组件包装在React `Router` Router的 [组件中](https://reacttraining.com/react-router/)。 AEM `ModelManager`SPA Editor JS SDK提供的动态路由会根据JSON模型API添加到AEM页面。

1. 打开终端，导航到项目的根，然后使用您的Maven技能将项目部署到AEM::

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 导航到AEM中的SPA主页： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 并打开浏览器的开发人员工具。 下面的截屏是从Google Chrome浏览器中捕获的。

   刷新页面，您应看到XHR请 `/content/wknd-spa-react/us/en.model.json`求，该请求是SPA根。 请注意，根据本教程前面提到的SPA根模板的层次结构深度配置，只包含三个子页面。 这不包括第 **3页**。

   ![初始JSON请求- SPA根](assets/navigation-routing/initial-json-request.png)

1. 打开开发人员工具后，使用 `Header` 导航导航到 **第3页**:

   ![第3页导航](assets/navigation-routing/page-three-navigation.png)

   请注意，新的XHR请求是： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager了解第3页 **JSON内容** 不可用，并自动触发额外的XHR请求。

1. 使用组件的各种导航链接继续导航 `Header` SPA。 请注意，未发出任何其他XHR请求，且未发生完整页面刷新。 这使最终用户的SPA变得更快，并减少了返回AEM的不必要请求。

   ![实现导航](assets/navigation-routing/final-navigation-implemented.gif)

1. 通过直接导航到： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 请注意，浏览器的“返回”按钮继续工作。

## 恭喜！ {#congratulations}

恭喜您，您学习了如何通过使用SPA编辑器SDK映射到AEM页面来支持SPA中的多个视图。 动态导航已使用React Router实现并添加到该组 `Header` 件中。

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ，或通过切换到分支在本地签出代码 `React/navigation-routing-solution`。
