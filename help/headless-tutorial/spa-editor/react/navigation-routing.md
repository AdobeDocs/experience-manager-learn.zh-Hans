---
title: 添加导航和路由 | AEM SPA Editor和React快速入门
description: 了解如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面，从而支持中的多个视图。 动态导航是使用React Router和React Core Components实现的。
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# 添加导航和路由 {#navigation-routing}

了解如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面，从而支持中的多个视图。 动态导航是使用React Router和React Core Components实现的。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
1. 了解使用 [React路由器](https://reacttraining.com/react-router/) 可在SPA的不同视图之间导航。
1. 使用AEM React核心组件实施由AEM页面层次结构驱动的动态导航。

## 将构建的内容

本章将向AEM中的SPA添加导航。 导航菜单由AEM页面层次结构驱动，并将利用 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![添加了导航](assets/navigation-routing/navigation-added.png)

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment). 本章是 [映射组件](map-components.md) 但是，章节之后，您只需要将一个启用了SPA的AEM项目部署到本地AEM实例。

## 将导航添加到模板 {#add-navigation-template}

1. 打开浏览器并登录AEM, [http://localhost:4502/](http://localhost:4502/). 应该已部署起始代码库。
1. 导航到 **SPA页面模板**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 选择最外侧 **根布局容器** 单击 **策略** 图标。 小心 **not** 选择 **布局容器** 未锁定进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

1. 创建名为的新策略 **SPA结构**:

   ![SPA结构策略](assets/navigation-routing/spa-policy-update.png)

   在 **允许的组件** > **常规** >选择 **布局容器** 组件。

   在 **允许的组件** > **WKND SPA REACT — 结构** >选择 **导航** 组件：

   ![选择导航组件](assets/navigation-routing/select-navigation-component.png)

   在 **允许的组件** > **WKND SPA REACT — 内容** >选择 **图像** 和 **文本** 组件。 您应选择总共4个组件。

   单击 **完成** 以保存更改。

1. 刷新页面，并添加 **导航** 未锁定组件上方的组件 **布局容器**:

   ![将导航组件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 选择 **导航** 组件，单击其 **策略** 图标以编辑策略。
1. 使用 **策略标题** of **SPA导航**.

   在 **属性**:

   * 设置 **导航根** to `/content/wknd-spa-react/us/en`.
   * 设置 **排除根级别** to **1**.
   * 取消选中 **收集所有子页面**.
   * 设置 **导航结构深度** to **3**.

   ![配置导航策略](assets/navigation-routing/navigation-policy.png)

   这将收集下方2层的导航 `/content/wknd-spa-react/us/en`.

1. 保存更改后，您应会看到填充的 `Navigation` 作为模板的一部分：

   ![填充的导航组件](assets/navigation-routing/populated-navigation.png)

## 创建子页面

接下来，在AEM中创建其他页面，以用作SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到 **站点** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 选择 **WKND SPA React主页** 单击 **创建** > **页面**:

   ![创建新页面](assets/navigation-routing/create-new-page.png)

1. 在 **模板** 选择 **SPA页面**. 在 **属性** enter **第1页** 对于 **标题** 和 **page-1** 作为名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击 **创建** 并在对话框弹出窗口中，单击 **打开** 以在AEM SPA编辑器中打开页面。

1. 添加新 **文本** 组件到主 **布局容器**. 编辑组件并输入文本： **第1页** 使用RTE和 **H2** 元素。

   ![示例内容页面1](assets/navigation-routing/page-1-sample-content.png)

   请随时添加其他内容，如图像。

1. 返回到AEM Sites控制台并重复上述步骤，从而创建另一个名为 **第2页** 作为 **第1页**.
1. 最后创建第三个页面， **第3页** 但作为 **孩子** of **第2页**. 完成网站层次结构后，应如下所示：

   ![网站层次结构示例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 导航组件现在可用于导航到SPA的不同区域。

   ![导航和路由](assets/navigation-routing/navigation-working.gif)

1. 在AEM编辑器之外打开页面： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 使用 **导航** 组件，以导航到应用程序的不同视图。

1. 在导航时，使用浏览器的开发人员工具检查网络请求。 下面的屏幕截图是从Google Chrome浏览器中捕获的。

   ![观察网络请求](assets/navigation-routing/inspect-network-requests.png)

   请注意，在初始页面加载后，后续导航不会导致页面完全刷新，并且在返回到之前访问的页面时，网络流量会最小化。

## 层级页面JSON模型 {#hierarchy-page-json-model}

接下来，检查可驱动SPA多视图体验的JSON模型。

1. 在新选项卡中，打开由AEM提供的JSON模型API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 使用浏览器扩展时，可能会对 [设置JSON格式](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   首次加载SPA时，将请求此JSON内容。 外部结构如下所示：

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

   在 `:children` 您应会看到创建的每个页面对应的条目。 所有页面的内容都位于此初始JSON请求中。 使用导航路由，可快速加载SPA的后续视图，因为内容已在客户端可用。

   加载不明智 **全部** 的JSON请求中SPA内容的URL，因为这会减慢初始页面加载速度。 接下来，我们将介绍如何收集页面的层次结构深度。

1. 导航到 **SPA根** 模板： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   单击 **页面属性菜单** > **页面策略**:

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

1. 的 **SPA根** 模板具有额外的 **分层结构** 选项卡来控制收集的JSON内容。 的 **结构深度** 确定网站层级中收集子页面的深度 **根**. 您还可以使用 **结构模式** 字段，以根据正则表达式筛选其他页面。

   更新 **结构深度** to **2**:

   ![更新结构深度](assets/navigation-routing/update-structure-depth.png)

   单击 **完成** 以保存对策略所做的更改。

1. 重新打开JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   请注意， **第3页** 路径已删除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 从初始JSON模型。 这是因为 **第3页** 位于层次结构的第3级，我们更新了策略，以仅包含级别2的最大深度的内容。

1. 重新打开SPA主页： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 并打开浏览器的开发人员工具。

   刷新页面，此时您应会看到 `/content/wknd-spa-react/us/en.model.json`，即SPA根。 请注意，根据本教程前面对SPA根模板进行的层次结构深度配置，只包含三个子页面。 这不包括 **第3页**.

   ![初始JSON请求 — SPA根](assets/navigation-routing/initial-json-request.png)

1. 打开开发人员工具后，使用 `Navigation` 组件直接导航到 **第3页**:

   请注意，新的XHR请求已发布到： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理器了解 **第3页** JSON内容不可用，并会自动触发其他XHR请求。

1. 通过直接导航到以下位置来体验深层链接： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 另请注意，浏览器的“返回”按钮可继续工作。

## Inspect React Routing  {#react-routing}

导航和路由通过 [React路由器](https://reactrouter.com/). React Router是React应用程序的导航组件集合。 [AEM React Core Components](https://github.com/adobe/aem-react-core-wcm-components-base) 使用React Router的功能来实施 **导航** 组件。

接下来，检查React Router与SPA的集成情况，并尝试使用React Router的 [链接](https://reactrouter.com/web/api/Link) 组件。

1. 在IDE中，打开文件 `index.js` at `ui.frontend/src/index.js`.

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

   请注意， `App` 包裹在 `Router` 组件从 [React路由器](https://reacttraining.com/react-router/). 的 `ModelManager`，由AEM SPA Editor JS SDK提供，会根据JSON模型API将动态路由添加到AEM页面。

1. 打开文件 `Page.js` at `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   的 `Page` SPA组件使用 `MapTo` 函数映射 **页面** 到相应的SPA组件。 的 `withRoute` 该实用程序可帮助根据 `cqPath` 属性。

1. 打开 `Header.js` 组件位置 `ui.frontend/src/components/Header/Header.js`.
1. 更新 `Header` 来包装 `<h1>` 标记 [链接](https://reactrouter.com/web/api/Link) 访问主页：

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   而不是使用默认 `<a>` 我们使用的锚点标记 `<Link>` 由React Router提供。 只要 `to=` 指向有效路由时，SPA将切换到该路由并 **not** 执行完整页面刷新。 在本例中，我们只需对指向主页的链接进行硬编码，以说明 `Link`.

1. 在以下位置更新测试： `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由于我们在 `App.js` 我们需要更新设备测试，以便考虑这一点。

1. 打开一个终端，导航到项目的根，然后使用您的Maven技能将项目部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在AEM中，导航到SPA中的一个页面： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   而不是使用 `Navigation` 组件，请在 `Header`.

   ![标题链接](assets/navigation-routing/header-link.png)

   请注意，完整的页面刷新为 **not** 触发且SPA路由正常工作。

1. 或者，尝试使用 `Header.js` 使用标准 `<a>` 锚点标记：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   这有助于说明SPA路由与常规网页链接之间的差异。

## 恭喜！ {#congratulations}

恭喜，您了解了如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面来支持这些视图。 动态导航已使用React Router实施并添加到 `Header` 组件。
