---
title: 将SPA组件映射到AEM组件 | AEM SPA编辑器和React快速入门
description: 了解如何使用AEM SPA编辑器JS SDK将React组件映射到Adobe Experience Manager (AEM)组件。 组件映射使用户能够在AEM SPA编辑器中对SPA组件进行动态更新，类似于传统的AEM创作。 您还将了解如何使用开箱即用的AEM React核心组件。
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 477
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# 将SPA组件映射到AEM组件 {#map-components}

{{spa-editor-deprecation}}

了解如何使用AEM SPA编辑器JS SDK将React组件映射到Adobe Experience Manager (AEM)组件。 组件映射使用户能够在AEM SPA编辑器中对SPA组件进行动态更新，类似于传统的AEM创作。

本章更深入地介绍了AEM JSON模型API，以及如何将由AEM组件公开的JSON内容作为prop自动插入到React组件中。

## 目标

1. 了解如何将AEM组件映射到SPA组件。
1. 检查React组件如何使用从AEM传递的动态属性。
1. 了解如何使用现成的[React AEM核心组件](https://github.com/adobe/aem-react-core-wcm-components-examples)。

## 您将构建的内容

本章检查提供的`Text` SPA组件如何映射到AEM `Text`组件。 在SPA中使用并在AEM中创作的React核心组件（如`Image` SPA组件）。 **布局容器**&#x200B;和&#x200B;**模板编辑器**&#x200B;策略的现成功能也用于创建外观变化稍大的视图。

![章节示例最终创作](./assets/map-components/final-page.png)

## 先决条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 本章是[集成SPA](integrate-spa.md)章节的延续，但您需要遵循的是一个支持SPA的AEM项目。

## 映射方法

基本概念是将SPA组件映射到AEM组件。 AEM组件运行服务器端，将内容导出为JSON模型API的一部分。 SPA使用在浏览器中运行客户端的JSON内容。 创建了SPA组件与AEM组件之间的1:1映射。

![将AEM组件映射到React组件的高级概述](./assets/map-components/high-level-approach.png)

*将AEM组件映射到React组件的高级概述*

## 检查文本组件

[AEM项目原型](https://github.com/adobe/aem-project-archetype)提供了一个映射到AEM [文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)的`Text`组件。 这是&#x200B;**content**&#x200B;组件的示例，该组件渲染来自AEM的&#x200B;*content*。

我们来看看组件的工作方式。

### 检查JSON模型

1. 在介绍SPA代码之前，请务必了解AEM提供的JSON模型。 导航到[核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html)并查看文本组件的页面。 核心组件库提供了所有AEM核心组件的示例。
1. 为以下示例之一选择&#x200B;**JSON**&#x200B;选项卡：

   ![文本JSON模型](./assets/map-components/text-json.png)

   您应该看到三个属性：`text`、`richText`和`:type`。

   `:type`是一个保留属性，它列出了AEM组件的`sling:resourceType`（或路径）。 `:type`的值用于将AEM组件映射到SPA组件。

   `text`和`richText`是对SPA组件公开的其他属性。

1. 在[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)处查看JSON输出。 您应该能够找到类似于以下内容的条目：

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### 检查文本SPA组件

1. 在您选择的IDE中，打开SPA的AEM项目。 展开`ui.frontend`模块并在`ui.frontend/src/components/Text/Text.js`下打开文件`Text.js`。

1. 我们将检查的第一个区域是位于~第40行的`class Text`：

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text`是标准React组件。 组件使用`this.props.richText`来确定要呈现的内容是富文本还是纯文本。 实际使用的“内容”来自`this.props.text`。

   为避免潜在的XSS攻击，富文本在使用[dangullySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)呈现内容之前通过`DOMPurify`进行转义。 在练习的前面从JSON模型中撤回`richText`和`text`属性。

1. 接下来，打开`ui.frontend/src/components/import-components.js`查看~第86行上的`TextEditConfig`：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述代码负责确定何时在AEM创作环境中呈现占位符。 如果`isEmpty`方法返回&#x200B;**true**，则会呈现占位符。

1. 最后，查看第94行以下位置的`MapTo`调用：

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo`由AEM SPA编辑器JS SDK (`@adobe/aem-react-editable-components`)提供。 路径`wknd-spa-react/components/text`表示AEM组件的`sling:resourceType`。 此路径与之前观察到的JSON模型公开的`:type`匹配。 `MapTo`负责解析JSON模型响应并将正确的值作为`props`传递到SPA组件。

   您可以在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`找到AEM `Text`组件定义。

## 使用React核心组件

[AEM WCM组件 — React核心实施](https://github.com/adobe/aem-react-core-wcm-components-base)和[AEM WCM组件 — Spa编辑器 — React核心实施](https://github.com/adobe/aem-react-core-wcm-components-spa)。 这些是一组可重复使用的UI组件，映射到开箱即用的AEM组件。 大多数项目可以重复使用这些组件作为自己的实施的起点。

1. 在项目代码中，打开位于`ui.frontend/src/components`的文件`import-components.js`。
此文件会导入映射到AEM组件的所有SPA组件。 考虑到SPA Editor实施的动态性质，我们必须显式引用与AEM可创作组件关联的任何SPA组件。 这允许AEM作者选择在应用程序中随处使用组件。
1. 以下import语句包含在项目中编写的SPA组件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. 存在来自`@adobe/aem-core-components-react-spa`和`@adobe/aem-core-components-react-base`的其他`imports`。 这些调用将导入React核心组件，并在当前项目中使其可用。 然后使用`MapTo`将这些组件映射到项目特定的AEM组件，就像前面的`Text`组件示例一样。

### 更新AEM策略

策略是AEM模板的一项功能，它让开发人员和高级用户能够精细地控制可以使用哪些组件。 React核心组件包含在SPA代码中，但需要通过策略启用，然后才能在应用程序中使用。

1. 从AEM开始屏幕导航到&#x200B;**工具** > **模板** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**。

1. 选择并打开&#x200B;**SPA页面**&#x200B;模板以进行编辑。

1. 选择&#x200B;**布局容器**&#x200B;并单击它的&#x200B;**策略**&#x200B;图标以编辑策略：

   ![布局容器策略](assets/map-components/edit-spa-page-template.png)

1. 在&#x200B;**允许的组件** > **WKND SPA React - Content** >检查&#x200B;**图像**、**Teaser**&#x200B;和&#x200B;**标题**&#x200B;下。

   ![更新的组件可用](assets/map-components/update-components-available.png)

   在&#x200B;**默认组件** > **添加映射**&#x200B;下，并选择&#x200B;**图像 — WKND SPA React - Content**&#x200B;组件：

   ![设置默认组件](./assets/map-components/default-components.png)

   输入`image/*`的&#x200B;**MIME类型**。

   单击&#x200B;**完成**&#x200B;以保存策略更新。

1. 在&#x200B;**布局容器**&#x200B;中，单击&#x200B;**文本**&#x200B;组件的&#x200B;**策略**&#x200B;图标。

   创建名为&#x200B;**WKND SPA Text**&#x200B;的新策略。 在&#x200B;**插件** > **格式** >下，选中所有框以启用其他格式选项：

   ![启用RTE格式](assets/map-components/enable-formatting-rte.png)

   在&#x200B;**插件** > **段落样式** >下，选中&#x200B;**启用段落样式**&#x200B;的框：

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   单击&#x200B;**完成**&#x200B;以保存策略更新。

### 创作内容

1. 导航到&#x200B;**主页** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

1. 您现在应该能够在页面上使用其他组件&#x200B;**Image**、**Teaser**&#x200B;和&#x200B;**Title**。

   ![其他组件](assets/map-components/additional-components.png)

1. 您还应该能够编辑`Text`组件并在&#x200B;**全屏**&#x200B;模式下添加其他段落样式。

   ![全屏富文本编辑](assets/map-components/full-screen-rte.png)

1. 您还应该能够从&#x200B;**资产查找器**&#x200B;中拖放图像：

   ![拖放图像](assets/map-components/drag-drop-image.png)

1. 使用&#x200B;**Title**&#x200B;和&#x200B;**Teaser**&#x200B;组件进行试验。

1. 通过[AEM Assets](http://localhost:4502/assets.html/content/dam)添加您自己的映像，或者为标准[WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest)安装完成的代码库。 [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest)包含可在WKND SPA上重复使用的许多图像。 可以使用[AEM的包管理器](http://localhost:4502/crx/packmgr/index.jsp)安装该包。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

## 检查布局容器

AEM SPA编辑器SDK自动提供对&#x200B;**布局容器**&#x200B;的支持。 名称指示的&#x200B;**布局容器**&#x200B;是&#x200B;**容器**&#x200B;组件。 容器组件是接受JSON结构的组件，该结构表示&#x200B;*其他*&#x200B;组件并动态实例化它们。

让我们进一步检查布局容器。

1. 在浏览器中导航到[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 响应式网格](./assets/map-components/responsive-grid-modeljson.png)

   **布局容器**&#x200B;组件具有`wcm/foundation/components/responsivegrid`的`sling:resourceType`，SPA编辑器使用`:type`属性识别它，就像`Text`和`Image`组件一样。

   在SPA编辑器中，可以使用[布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新调整组件大小的相同功能。

2. 返回到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 添加其他&#x200B;**图像**&#x200B;组件，然后尝试使用&#x200B;**布局**&#x200B;选项重新调整其大小：

   ![使用布局模式重新调整图像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新打开JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)并观察作为JSON一部分的`columnClassNames`：

   ![列类名](./assets/map-components/responsive-grid-classnames.png)

   类名`aem-GridColumn--default--4`指示组件应基于12列网格为4列宽。 有关[响应式网格的更多详细信息见此处](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回到IDE，在`ui.apps`模块中`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`处定义了客户端库。 打开文件`less/grid.less`。

   此文件确定&#x200B;**布局容器**&#x200B;使用的断点（`default`、`tablet`和`phone`）。 此文件将根据项目规范进行自定义。 当前断点设置为`1200px`和`768px`。

5. 您应该能够使用`Text`组件的响应式功能和更新的富文本策略来创作类似于以下内容的视图：

   ![章节示例最终创作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您已了解如何将SPA组件映射到AEM组件，并且您使用了React核心组件。 您还有机会探索&#x200B;**布局容器**&#x200B;的响应式功能。

### 后续步骤 {#next-steps}

[导航和路由](navigation-routing.md) — 了解如何使用SPA Editor SDK映射到AEM Pages，从而支持SPA中的多个视图。 动态导航是使用React Router和React Core Components实现的。

## （额外练习）将配置保留到源代码管理 {#bonus-configs}

在许多情况下，尤其是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源代码控制中很有价值。 这可确保所有开发人员都针对同一组内容和配置工作，并可确保环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的操作就可以交给一组特殊的超级用户。

接下来的几个步骤将使用Visual Studio Code IDE和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)执行，但可以使用任何工具和您已配置为从AEM的本地实例&#x200B;**提取**&#x200B;或&#x200B;**导入**&#x200B;内容的任何IDE执行。

1. 在Visual Studio Code IDE中，确保已通过Marketplace扩展安装&#x200B;**VSCode AEM Sync**：

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 在项目资源管理器中展开&#x200B;**ui.content**&#x200B;模块并导航到`/conf/wknd-spa-react/settings/wcm/templates`。

3. **右键单击** `templates`文件夹并选择&#x200B;**从AEM服务器导入**：

   ![VSCode导入模板](./assets/map-components/import-aem-servervscode.png)

4. 重复导入内容的步骤，但选择位于`/conf/wknd-spa-react/settings/wcm/templates/policies`的&#x200B;**策略**&#x200B;文件夹。

5. 检查位于`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`文件。

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   `filter.xml`文件负责识别与包一起安装的节点的路径。 请注意每个筛选器上的`mode="merge"`，这表示现有内容将不会被修改，而是只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署&#x200B;**不**&#x200B;覆盖内容非常重要。 有关使用筛选器元素的更多详细信息，请参阅[FileVault文档](https://jackrabbit.apache.org/filevault/filter.html)。

   比较`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每个模块管理的不同节点。

## （额外练习）创建自定义图像组件 {#bonus-image}

React核心组件已提供了SPA图像组件。 但是，如果您需要额外的练习，请创建自己的映射到AEM [图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)的React实施。 `Image`组件是&#x200B;**content**&#x200B;组件的另一个示例。

### 检查JSON

在跳转到SPA代码之前，请检查AEM提供的JSON模型。

1. 导航到核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html)中的[图像示例。

   ![图像核心组件JSON](./assets/map-components/image-json.png)

   `src`、`alt`和`title`的属性用于填充SPA `Image`组件。

   >[!NOTE]
   >
   > 其他公开的图像属性(`lazyEnabled`、`widths`)允许开发人员创建自适应和延迟加载组件。 本教程中构建的组件非常简单，**不**&#x200B;会使用这些高级属性。

### 实施图像组件

1. 接下来，在`ui.frontend/src/components`下创建一个名为`Image`的新文件夹。
1. 在`Image`文件夹下创建名为`Image.js`的新文件。

   ![Image.js文件](./assets/map-components/image-js-file.png)

1. 将以下`import`语句添加到`Image.js`：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 然后添加`ImageEditConfig`以确定何时在AEM中显示占位符：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未设置`src`属性，将显示占位符。

1. 接下来实施`Image`类：

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   上述代码将根据JSON模型传入的prop `src`、`alt`和`title`呈现`<img>`。

1. 添加`MapTo`代码以将React组件映射到AEM组件：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   请注意，字符串`wknd-spa-react/components/image`对应于AEM组件在`ui.apps`中的位置： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

1. 在同一目录中创建一个名为`Image.css`的新文件并添加以下内容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. 在`Image.js`中，在`import`语句下方的顶部添加对文件的引用：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. 打开文件`ui.frontend/src/components/import-components.js`并添加对新`Image`组件的引用：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. 在`import-components.js`中注释掉React核心组件图像：

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   这将确保改用我们的自定义图像组件。

1. 使用Maven将SPA代码从项目的根部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在AEM中检查SPA。 页面上的任何图像组件都应继续工作。 检查渲染的输出，您应该会看到自定义图像组件的标记，而不是React核心组件。

   *自定义图像组件标记*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *React核心组件图像标记*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   这是扩展和实施您自己的组件的良好介绍。
