---
title: 添加导航和路由 | AEM SPA Editor和React快速入门
description: 了解如何使用SPA编辑器SDK将映射到AEM页面，从而支持SPA中的多个视图。 动态导航是使用React Router和React核心组件实现的。
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

了解如何使用SPA编辑器SDK将映射到AEM页面，从而支持SPA中的多个视图。 动态导航是使用React Router和React核心组件实现的。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
1. 了解如何使用 [React路由器](https://reacttraining.com/react-router/) 在SPA的不同视图之间导航。
1. 使用AEM React核心组件实施由AEM页面层次结构驱动的动态导航。

## 您将构建的内容

本章将导航添加到AEM中的SPA。 导航菜单由AEM页面层次结构驱动，将利用提供的JSON模型 [导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![已添加导航](assets/navigation-routing/navigation-added.png)

## 前提条件

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment). 本章是 [映射组件](map-components.md) 但是，接下来要遵循的章节是一个部署到本地SPA实例的启用AEM的AEM项目。

## 将导航添加到模板 {#add-navigation-template}

1. 打开浏览器并登录AEM， [http://localhost:4502/](http://localhost:4502/). 起始代码库应已部署。
1. 导航到 **SPA页面模板**： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 选择最外层的 **根布局容器** 并单击其 **策略** 图标。 小心点 **非** 以选择 **布局容器** 已解锁进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

1. 创建新策略，命名为 **SPA结构**：

   ![SPA结构策略](assets/navigation-routing/spa-policy-update.png)

   下 **允许的组件** > **常规** >选择 **布局容器** 组件。

   下 **允许的组件** > **WKND SPA REACT — 结构** >选择 **导航** 组件：

   ![选择导航组件](assets/navigation-routing/select-navigation-component.png)

   下 **允许的组件** > **WKND SPA REACT — 内容** >选择 **图像** 和 **文本** 组件。 您总共应选择4个组件。

   单击&#x200B;**完成**&#x200B;以保存更改。

1. 刷新页面，然后添加 **导航** 组件位于解锁位置上方 **布局容器**：

   ![将导航组件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 选择 **导航** 组件并单击其 **策略** 图标以编辑策略。
1. 使用创建新策略 **策略标题** 之 **SPA导航**.

   在 **属性**：

   * 设置 **导航根目录** 到 `/content/wknd-spa-react/us/en`.
   * 设置 **排除根级别** 到 **1**.
   * 取消选中 **收集所有子页面**.
   * 设置 **导航结构深度** 到 **3**.

   ![配置导航策略](assets/navigation-routing/navigation-policy.png)

   这将收集下方2个级别的导航 `/content/wknd-spa-react/us/en`.

1. 保存更改后，您应会看到已填充 `Navigation` 作为模板的一部分：

   ![填充的导航组件](assets/navigation-routing/populated-navigation.png)

## 创建子页面

接下来，在AEM中创建其他页面，这些页面将用作SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到 **站点** 控制台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 选择 **WKND SPA React主页** 并单击 **创建** > **页面**：

   ![创建新页面](assets/navigation-routing/create-new-page.png)

1. 下 **模板** 选择 **SPA页面**. 下 **属性** 进入 **第1页** 对于 **标题** 和 **page-1** 作为名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击 **创建** 在对话框弹出窗口中，单击 **打开** 以在AEM SPA编辑器中打开该页面。

1. 添加新 **文本** 组件到主 **布局容器**. 编辑组件并输入文本： **第1页** 使用RTE和 **H2** 元素。

   ![示例内容页面1](assets/navigation-routing/page-1-sample-content.png)

   您可以随意添加其他内容，如图像。

1. 返回到AEM Sites控制台并重复上述步骤，创建名为的第二个页面 **第2页** 作为兄弟姐妹 **第1页**.
1. 最后，创建第三个页面， **第3页** 但作为 **子项** 之 **第2页**. 完成之后，站点层次结构应如下所示：

   ![示例站点层次结构](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 导航组件现在可用于导航到SPA的不同区域。

   ![导航和路由](assets/navigation-routing/navigation-working.gif)

1. 在AEM编辑器外部打开页面： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 使用 **导航** 导航到应用程序的不同视图。

1. 导航时，使用浏览器的开发人员工具检查网络请求。 下面的屏幕截图是从Google Chrome浏览器中捕获的。

   ![观察网络请求](assets/navigation-routing/inspect-network-requests.png)

   请注意，在初始页面加载后，后续导航不会导致完全页面刷新，并且当返回到先前访问的页面时，网络通信量会降至最低。

## 层次结构页面JSON模型 {#hierarchy-page-json-model}

接下来，检查驱动SPA多视图体验的JSON模型。

1. 在新选项卡中，打开由AEM提供的JSON模型API： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 使用浏览器扩展执行以下操作可能会很有帮助 [设置JSON的格式](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   首次加载SPA时会请求此JSON内容。 外部结构如下所示：

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

   下 `:children` 您应该会看到每个已创建页面的条目。 所有页面的内容都包含在此初始JSON请求中。 通过导航路由，SPA的后续视图会快速加载，因为内容在客户端已经可用。

   装载是不明智的 **全部** 初始JSON请求中SPA内容的限制，因为这会减慢初始页面加载的速度。 接下来，让我们查看如何收集页面的层次结构深度。

1. 导航到 **SPA根目录** 模板位于： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   单击 **页面属性菜单** > **页面策略**：

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

1. 此 **SPA根目录** 模板有一个额外的 **层次结构** 选项卡来控制收集的JSON内容。 此 **结构深度** 确定网站层次结构中收集子页面的深度 **根**. 您还可以使用 **结构模式** 用于根据正则表达式筛选掉其他页面的字段。

   更新 **结构深度** 到 **2**：

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

   请注意 **第3页** 路径已删除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 来自初始JSON模型。 这是因为 **第3页** 位于层级中的第3级，我们更新了策略以仅包含最大深度为第2级的内容。

1. 重新打开SPA主页： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 并打开浏览器的开发人员工具。

   刷新该页，您应该会看到以下内容的XHR请求 `/content/wknd-spa-react/us/en.model.json`，即SPA根。 请注意，根据教程中前面对SPA根模板进行的层级深度配置，只包含三个子页面。 这不包括 **第3页**.

   ![初始JSON请求 — SPA根目录](assets/navigation-routing/initial-json-request.png)

1. 在开发人员工具打开时，使用 `Navigation` 要直接导航到的组件 **第3页**：

   请注意，已发出新的XHR请求以： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理器了解 **第3页** JSON内容不可用，并自动触发其他XHR请求。

1. 尝试直接导航到以下位置来访问深层链接： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 另请注意，浏览器的“后退”按钮将继续工作。

## Inspect React路由  {#react-routing}

导航和路由的实现方式 [React路由器](https://reactrouter.com/). React Router是React应用程序导航组件的集合。 [AEM React核心组件](https://github.com/adobe/aem-react-core-wcm-components-base) 使用React Router的功能实现 **导航** 之前步骤中使用的组件。

接下来，检查React Router如何与SPA集成，并使用React Router的 [链接](https://reactrouter.com/web/api/Link) 组件。

1. 在IDE中打开文件 `index.js` 在 `ui.frontend/src/index.js`.

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

   请注意 `App` 包在 `Router` 组件来源 [React路由器](https://reacttraining.com/react-router/). 此 `ModelManager`由AEM SPA编辑器JS SDK提供，基于JSON模型API向AEM页面添加动态路由。

1. 打开文件 `Page.js` 在 `ui.frontend/src/components/Page/Page.js`

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

   此 `Page` SPA组件使用 `MapTo` 映射函数 **页面** 到AEM中的相应SPA组件。 此 `withRoute` SPA AEM实用程序有助于根据 `cqPath` 属性。

1. 打开 `Header.js` 组件位于 `ui.frontend/src/components/Header/Header.js`.
1. 更新 `Header` 以包住 `<h1>` 标记中 [链接](https://reactrouter.com/web/api/Link) 到主页：

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

   不要使用默认值 `<a>` 我们使用的锚点标记 `<Link>` 由React路由器提供。 只要 `to=` 指向有效路由，SPA将切换到该路由，并且 **非** 执行全页刷新。 在这里，我们只是对指向主页的链接进行硬编码以说明如何使用 `Link`.

1. 更新测试于 `App.test.js` 在 `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由于我们是在中引用的静态组件中使用React Router的功能 `App.js` 我们需要更新单元测试以考虑这一点。

1. 打开终端，导航到项目的根目录，然后使用您的Maven技能将该项目部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 导航到AEM中的SPA中的以下页面之一： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   不要使用 `Navigation` 要导航的组件，请使用 `Header`.

   ![标题链接](assets/navigation-routing/header-link.png)

   请注意，完整页面刷新是 **非** 触发，且SPA路由运行正常。

1. （可选）使用 `Header.js` 使用标准格式的文件 `<a>` 锚点标记：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   这有助于说明SPA路由与常规网页链接之间的区别。

## 恭喜！ {#congratulations}

恭喜，您已了解如何使用SPA编辑器SDK将映射到AEM页面，从而支持SPA中的多个视图。 已使用React Router实施动态导航，并将其添加到 `Header` 组件。
