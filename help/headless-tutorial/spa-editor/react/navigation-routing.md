---
title: 添加导航和路由 | AEM SPA Editor和React快速入门
description: 了解如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面，从而支持中的多个视图。 动态导航是使用React Router和React Core Components实现的。
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1619'
ht-degree: 0%

---

# 添加导航和路由 {#navigation-routing}

了解如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面，从而支持中的多个视图。 动态导航是使用React Router和React Core Components实现的。

## 目标

1. 了解使用SPA编辑器时可用的SPA模型路由选项。
1. 了解如何使用[React Router](https://reacttraining.com/react-router/)在SPA的不同视图之间导航。
1. 使用AEM React核心组件实施由AEM页面层次结构驱动的动态导航。

## 将构建的内容

本章将向AEM中的SPA添加导航。 导航菜单将由AEM页面层次结构驱动，并将利用[导航核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html)提供的JSON模型。

![添加了导航](assets/navigation-routing/navigation-added.png)

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 本章是[映射组件](map-components.md)章节的继续，但是，随后您只需要将一个启用了SPA的AEM项目部署到本地AEM实例。

## 将导航添加到模板 {#add-navigation-template}

1. 打开浏览器并登录AEM, [http://localhost:4502/](http://localhost:4502/)。 应该已部署起始代码库。
1. 导航到&#x200B;**SPA页面模板**:[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)。
1. 选择最外部的&#x200B;**根布局容器**&#x200B;并单击其&#x200B;**策略**&#x200B;图标。 要小心&#x200B;**不要**&#x200B;选择&#x200B;**布局容器**&#x200B;未锁定进行创作。

   ![选择根布局容器策略图标](assets/navigation-routing/root-layout-container-policy.png)

1. 创建名为&#x200B;**SPA Structure**&#x200B;的新策略：

   ![SPA结构策略](assets/navigation-routing/spa-policy-update.png)

   在&#x200B;**允许的组件** > **常规** >下，选择&#x200B;**布局容器**&#x200B;组件。

   在&#x200B;**允许的组件** > **WKND SPA REACT - STRUCTURE**&#x200B;下，选择&#x200B;**导航**&#x200B;组件：

   ![选择导航组件](assets/navigation-routing/select-navigation-component.png)

   在&#x200B;**允许的组件** > **WKND SPA REACT — 内容**>下，选择&#x200B;**图像**&#x200B;和&#x200B;**文本**&#x200B;组件。 您应选择总共4个组件。

   单击&#x200B;**完成**&#x200B;以保存更改。

1. 刷新页面，并在未锁定的&#x200B;**布局容器**&#x200B;上方添加&#x200B;**导航**&#x200B;组件：

   ![将导航组件添加到模板](assets/navigation-routing/add-navigation-component.png)

1. 选择&#x200B;**导航**&#x200B;组件并单击其&#x200B;**策略**&#x200B;图标以编辑策略。
1. 使用&#x200B;**SPA Navigation**&#x200B;的&#x200B;**策略标题**&#x200B;创建新策略。

   在&#x200B;**Properties**&#x200B;下：

   * 将&#x200B;**导航根**&#x200B;设置为`/content/wknd-spa-react/us/en`。
   * 将&#x200B;**排除根级别**&#x200B;设置为&#x200B;**1**。
   * 取消选中&#x200B;**收集所有子页面**。
   * 将&#x200B;**导航结构深度**&#x200B;设置为&#x200B;**3**。

   ![配置导航策略](assets/navigation-routing/navigation-policy.png)

   这将收集`/content/wknd-spa-react/us/en`下方深处的导航2级。

1. 保存更改后，您应会看到填充的`Navigation`作为模板的一部分：

   ![填充的导航组件](assets/navigation-routing/populated-navigation.png)

## 创建子页面

接下来，在AEM中创建其他页面，以用作SPA中的不同视图。 我们还将检查AEM提供的JSON模型的层次结构。

1. 导航到&#x200B;**Sites**&#x200B;控制台：[http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home)。 选择&#x200B;**WKND SPA React主页**&#x200B;并单击&#x200B;**创建** > **页面**:

   ![创建新页面](assets/navigation-routing/create-new-page.png)

1. 在&#x200B;**Template**&#x200B;下，选择&#x200B;**SPA Page**。 在&#x200B;**Properties**&#x200B;下，输入&#x200B;**Title**&#x200B;和&#x200B;**page-1**&#x200B;的&#x200B;**Page 1**&#x200B;作为名称。

   ![输入初始页面属性](assets/navigation-routing/initial-page-properties.png)

   单击&#x200B;**创建**，然后在对话框弹出窗口中，单击&#x200B;**打开**&#x200B;以在AEM SPA编辑器中打开该页面。

1. 向主&#x200B;**布局容器**&#x200B;添加新的&#x200B;**Text**&#x200B;组件。 编辑组件并输入文本：**使用RTE和** H2 **元素的第1**&#x200B;页。

   ![示例内容页面1](assets/navigation-routing/page-1-sample-content.png)

   请随时添加其他内容，如图像。

1. 返回到AEM Sites控制台并重复上述步骤，创建名为&#x200B;**Page 2**&#x200B;的第二个页面，作为&#x200B;**Page 1**&#x200B;的同级页面。
1. 最后，创建第三页，**第3**&#x200B;页，但作为&#x200B;**第2页**&#x200B;的&#x200B;**子**&#x200B;创建。 完成网站层次结构后，应如下所示：

   ![网站层次结构示例](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 导航组件现在可用于导航到SPA的不同区域。

   ![导航和路由](assets/navigation-routing/navigation-working.gif)

1. 在AEM编辑器之外打开页面：[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)。 使用&#x200B;**导航**&#x200B;组件导航到应用程序的不同视图。

1. 在导航时，使用浏览器的开发人员工具检查网络请求。 下面的屏幕截图是从Google Chrome浏览器中捕获的。

   ![观察网络请求](assets/navigation-routing/inspect-network-requests.png)

   请注意，在初始页面加载后，后续导航不会导致页面完全刷新，并且在返回到之前访问的页面时，网络流量会最小化。

## 层级页面JSON模型 {#hierarchy-page-json-model}

接下来，检查可驱动SPA多视图体验的JSON模型。

1. 在新选项卡中，打开由AEM提供的JSON模型API:[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 使用浏览器扩展设置JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)格式可能会很有帮助。[

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

   在`:children`下，您应会看到每个已创建页面的条目。 所有页面的内容都位于此初始JSON请求中。 使用导航路由，将快速加载SPA的后续视图，因为内容已在客户端可用。

   在初始JSON请求中加载SPA内容的&#x200B;**ALL**&#x200B;并不明智，因为这会减缓初始页面加载速度。 接下来，我们将介绍如何收集页面的层次结构深度。

1. 导航到&#x200B;**SPA根**&#x200B;模板：[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html)。

   单击&#x200B;**页面属性菜单** > **页面策略**:

   ![打开SPA根的页面策略](assets/navigation-routing/open-page-policy.png)

1. **SPA根**&#x200B;模板具有额外的&#x200B;**分层结构**&#x200B;选项卡，用于控制收集的JSON内容。 **结构深度**&#x200B;确定在站点层次结构中收集&#x200B;**根**&#x200B;下子页面的深度。 您还可以使用&#x200B;**结构模式**&#x200B;字段根据正则表达式过滤掉其他页面。

   将&#x200B;**结构深度**&#x200B;更新为&#x200B;**2**:

   ![更新结构深度](assets/navigation-routing/update-structure-depth.png)

   单击&#x200B;**完成**&#x200B;以保存对策略所做的更改。

1. 重新打开JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

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

   请注意，**Page 3**&#x200B;路径已被删除：`/content/wknd-spa-react/us/en/home/page-2/page-3`。 这是因为&#x200B;**页面3**&#x200B;位于层次结构的第3级，我们更新了策略，以仅包含最大深度为第2级的内容。

1. 重新打开SPA主页：[http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html)并打开浏览器的开发人员工具。

   刷新页面后，您应会看到对`/content/wknd-spa-react/us/en.model.json`的XHR请求，该请求是SPA根。 请注意，根据本教程前面对SPA根模板进行的层次结构深度配置，只包含三个子页面。 这不包括&#x200B;**第3**&#x200B;页。

   ![初始JSON请求 — SPA根](assets/navigation-routing/initial-json-request.png)

1. 打开开发人员工具后，使用`Navigation`组件直接导航到&#x200B;**第3**&#x200B;页：

   请注意，新的XHR请求已发布到：`/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3页XHR请求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理器了解&#x200B;**第3**&#x200B;页JSON内容不可用，并自动触发其他XHR请求。

1. 通过直接导航到以下位置来体验深层链接：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html)。 另请注意，浏览器的“返回”按钮可继续工作。

## Inspect React Routing  {#react-routing}

导航和路由采用[React Router](https://reactrouter.com/)实现。 React Router是React应用程序的导航组件集合。 [AEM React核心组](https://github.com/adobe/aem-react-core-wcm-components-base) 件使用React Router的功能来实施 **** 前面步骤中使用的Navigation组件。

接下来，检查React Router与SPA的集成方式，并试用React Router的[Link](https://reactrouter.com/web/api/Link)组件。

1. 在IDE中，在`ui.frontend/src/index.js`处打开文件`index.js`。

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

   请注意，`App`封装在[React Router](https://reacttraining.com/react-router/)的`Router`组件中。 `ModelManager`由AEM SPA Editor JS SDK提供，它会根据JSON模型API将动态路由添加到AEM页面。

1. 在`ui.frontend/src/components/Page/Page.js`处打开文件`Page.js`

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

   `Page` SPA组件使用`MapTo`函数将AEM中的&#x200B;**Pages**&#x200B;映射到相应的SPA组件。 `withRoute`实用程序有助于根据`cqPath`属性将SPA动态路由到相应的AEM子页面。

1. 在`ui.frontend/src/components/Header/Header.js`处打开`Header.js`组件。
1. 更新`Header`以将`<h1>`标记包含在[Link](https://reactrouter.com/web/api/Link)中，以包装到主页：

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

   我们使用的不是React Router提供的`<Link>`默认的`<a>`锚点标记。 只要`to=`指向有效路由，SPA就会切换到该路由，并且&#x200B;**not**&#x200B;会执行全页刷新。 在本例中，我们只需对指向主页的链接进行硬编码，以说明`Link`的用法。

1. 在`App.test.js`的`ui.frontend/src/App.test.js`更新测试。

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由于我们在`App.js`中引用的静态组件中使用React Router的功能，因此需要更新设备测试以考虑这一点。

1. 打开一个终端，导航到项目的根，然后使用您的Maven技能将项目部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在AEM中，导航到SPA中的一个页面：[http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   使用`Header`中的链接，而不是使用`Navigation`组件进行导航。

   ![标题链接](assets/navigation-routing/header-link.png)

   请注意，完整的页面刷新是&#x200B;**未**&#x200B;触发的，并且SPA路由正在工作。

1. 或者，也可以使用标准`<a>`锚点标记来试验`Header.js`文件：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   这有助于说明SPA路由与常规网页链接之间的差异。

## 恭喜！ {#congratulations}

恭喜，您了解了如何通过SPA Editor SDK将SPA中的多个视图映射到AEM页面来支持这些视图。 动态导航已使用React Router实现并添加到`Header`组件中。
