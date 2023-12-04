---
title: 将SPA组件映射到AEM组件 | AEM SPA Editor和React快速入门
description: 了解如何使用AEM SPA编辑器JS SDK将React组件映射到Adobe Experience Manager (AEM)组件。 组件映射使用户能够对AEM SPA编辑器中的SPA组件进行动态更新，这与传统AEM创作类似。 您还将了解如何使用现成的AEM React核心组件。
feature: SPA Editor
version: Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 657
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# 将SPA组件映射到AEM组件 {#map-components}

了解如何使用AEM SPA编辑器JS SDK将React组件映射到Adobe Experience Manager (AEM)组件。 组件映射使用户能够对AEM SPA编辑器中的SPA组件进行动态更新，这与传统AEM创作类似。

本章更深入地介绍了AEM JSON模型API，以及如何将由AEM组件公开的JSON内容作为prop自动插入到React组件中。

## 目标

1. 了解如何将AEM组件映射到SPA组件。
1. Inspect React组件如何使用从AEM传递的动态属性。
1. 了解如何开箱即用 [React AEM核心组件](https://github.com/adobe/aem-react-core-wcm-components-examples).

## 您将构建的内容

本章将检查提供的 `Text` SPA组件已映射到AEM `Text`组件。 React核心组件，如 `Image` SPA组件在SPA中使用并在AEM中创作。 的开箱即用功能 **布局容器** 和 **模板编辑器** 策略还用于创建外观变化稍大的视图。

![章节示例最终创作](./assets/map-components/final-page.png)

## 前提条件

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment). 本章是 [集成SPA](integrate-spa.md) 但是，章节是启用SPA的AEM项目，您只需关注该章节。

## 映射方法

基本概念是将SPA组件映射到AEM组件。 AEM组件运行服务器端，将内容导出为JSON模型API的一部分。 JSON内容由SPA使用，在浏览器中运行客户端。 在SPA组件和AEM组件之间创建1:1映射。

![将AEM组件映射到React组件的高级概述](./assets/map-components/high-level-approach.png)

*将AEM组件映射到React组件的高级概述*

## Inspect文本组件

此 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 提供 `Text` 映射到AEM的组件 [文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 以下示例为 **内容** 组件，在中，它呈现 *内容* 来自AEM的。

我们来看看组件的工作方式。

### Inspect的JSON模型

1. 在跳转到SPA代码之前，请务必了解AEM提供的JSON模型。 导航至 [核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 和查看文本组件的页面。 核心组件库提供了所有AEM核心组件的示例。
1. 选择 **JSON** 选项卡以了解以下示例之一：

   ![文本JSON模型](./assets/map-components/text-json.png)

   您应该会看到三个属性： `text`， `richText`、和 `:type`.

   `:type` 是一个保留属性，其中列出了 `sling:resourceType` AEM组件的（或路径）。 的值 `:type` 是用于将AEM组件映射到SPA组件的功能。

   `text` 和 `richText` 是公开给SPA组件的其他属性。

1. 在以下位置查看JSON输出： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 您应该能够找到类似于以下内容的条目：

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect文本SPA组件

1. 在您选择的IDE中，打开SPA的AEM项目。 展开 `ui.frontend` 模块并打开文件 `Text.js` 下 `ui.frontend/src/components/Text/Text.js`.

1. 我们将检查的第一个区域是 `class Text` 在第40行：

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

   `Text` 是标准React组件。 组件使用 `this.props.richText` 确定要渲染的内容是富文本还是纯文本。 实际使用的“内容”来自 `this.props.text`.

   为了避免潜在的XSS攻击，富文本通过以下方式转义： `DOMPurify` 使用前 [dangallySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 以呈现内容。 撤消 `richText` 和 `text` JSON模型中的属性。

1. 下一步，打开 `ui.frontend/src/components/import-components.js` 查看 `TextEditConfig` 在第86行：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述代码负责确定何时在AEM创作环境中呈现占位符。 如果 `isEmpty` 方法返回 **true** 然后会呈现占位符。

1. 最后，查看 `MapTo` 拨打到第94行：

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` 由AEM SPA编辑器JS SDK提供(`@adobe/aem-react-editable-components`)。 路径 `wknd-spa-react/components/text` 表示 `sling:resourceType` AEM的URL值。 此路径与 `:type` 已由之前观察到的JSON模型公开。 `MapTo` 负责解析JSON模型响应并将正确的值传递为 `props` 到SPA组件。

   您可以找到AEM `Text` 组件定义位于 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## 使用React核心组件

[AEM WCM组件 — React核心实施](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM WCM组件 — Spa编辑器 — React Core实施](https://github.com/adobe/aem-react-core-wcm-components-spa). 这是一组可重复使用的UI组件，这些组件映射到开箱即用的AEM组件。 大多数项目可以重复使用这些组件作为自己的实施的起点。

1. 在项目代码中打开文件 `import-components.js` 在 `ui.frontend/src/components`.
此文件会导入映射到SPA组件的所有AEM组件。 考虑到SPA编辑器实施的动态性质，我们必须显式引用任何与SPA可创作组件绑定的AEM组件。 这允许AEM作者选择在应用程序中随处使用组件。
1. 以下import语句包含在项目中编写的SPA组件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. 还有其他几个 `imports` 从 `@adobe/aem-core-components-react-spa` 和 `@adobe/aem-core-components-react-base`. 这些调用将导入React核心组件，并在当前项目中使其可用。 然后，使用将这些组件映射到项目特定的AEM组件。 `MapTo`，就像使用 `Text` 组件示例（上文）。

### 更新AEM策略

策略是AEM模板的一项功能，它让开发人员和高级用户能够精细地控制可以使用哪些组件。 React核心组件包含在SPA代码中，但需要通过策略启用，然后才能在应用程序中使用。

1. 从AEM开始屏幕导航到 **工具** > **模板** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. 选择并打开 **SPA页面** 用于编辑的模板。

1. 选择 **布局容器** 单击它 **策略** 图标以编辑策略：

   ![布局容器策略](assets/map-components/edit-spa-page-template.png)

1. 下 **允许的组件** > **WKND SPA React — 内容** >检查 **图像**， **Teaser**、和 **标题**.

   ![更新的可用组件](assets/map-components/update-components-available.png)

   下 **默认组件** > **添加映射** 并选择 **图像 — WKND SPA React — 内容** 组件：

   ![设置默认组件](./assets/map-components/default-components.png)

   输入 **mime类型** 之 `image/*`.

   单击 **完成** 以保存策略更新。

1. 在 **布局容器** 单击 **策略** 图标 **文本** 组件。

   创建新策略，名为 **WKND SPA文本**. 下 **插件** > **格式化** >选中所有框以启用其他格式选项：

   ![启用RTE格式](assets/map-components/enable-formatting-rte.png)

   下 **插件** > **段落样式** >选中 **启用段落样式**：

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   单击 **完成** 以保存策略更新。

### 创作内容

1. 导航至 **主页** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. 现在，您应该能够使用其他组件 **图像**， **Teaser**、和 **标题** 在页面上。

   ![其他组件](assets/map-components/additional-components.png)

1. 您还应该能够编辑 `Text` 组件并在中添加其他段落样式 **全屏** 模式。

   ![全屏富文本编辑](assets/map-components/full-screen-rte.png)

1. 您还应该能够从 **资产查找器**：

   ![拖放图像](assets/map-components/drag-drop-image.png)

1. 使用进行试验 **标题** 和 **Teaser** 组件。

1. 通过以下方式添加您自己的图像 [AEM Assets](http://localhost:4502/assets.html/content/dam) 或安装完成的标准代码库 [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest). 此 [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 包括可在WKND SPA上重复使用的许多图像。 可使用以下方式安装软件包 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect布局容器

支持 **布局容器** 由AEM SPA编辑器SDK自动提供。 此 **布局容器**&#x200B;如名称所示，是 **容器** 组件。 容器组件是接受JSON结构的组件，表示 *其他* 组件并动态实例化它们。

让我们进一步检查布局容器。

1. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 响应式网格](./assets/map-components/responsive-grid-modeljson.png)

   此 **布局容器** 组件具有 `sling:resourceType` 之 `wcm/foundation/components/responsivegrid` 并由SPA编辑器使用 `:type` 属性，就像 `Text` 和 `Image` 组件。

   使用重新调整组件大小的相同功能 [布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 可用于SPA编辑器。

2. 返回到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 添加其他 **图像** 组件，然后尝试使用 **布局** 选项：

   ![使用布局模式重新调整图像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新打开JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 并观察 `columnClassNames` 作为JSON的一部分：

   ![列类名](./assets/map-components/responsive-grid-classnames.png)

   类名称 `aem-GridColumn--default--4` 指示组件应基于12列网格为4列宽。 更多有关 [可在此处找到响应式网格](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. 返回到IDE并在 `ui.apps` 模块在上定义了客户端库 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. 打开文件 `less/grid.less`.

   此文件确定断点(`default`， `tablet`、和 `phone`)，用于 **布局容器**. 此文件将根据项目规范进行自定义。 当前，断点设置为 `1200px` 和 `768px`.

5. 您应该能够使用的响应式功能和更新的富文本策略 `Text` 用于创作视图的组件，如下所示：

   ![章节示例最终创作](assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您已了解如何将SPA组件映射到AEM组件，并且使用了React核心组件。 您还有机会探索的响应式功能 **布局容器**.

### 后续步骤 {#next-steps}

[导航和路由](navigation-routing.md)  — 了解如何使用SPA编辑器SDK将映射到AEM页面，从而支持SPA中的多个视图。 动态导航是使用React Router和React Core Components实现的。

## （额外练习）将配置保留到源代码管理 {#bonus-configs}

在许多情况下，尤其是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源代码控制中非常有用。 这可确保所有开发人员都针对同一组内容和配置工作，并可确保环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的操作就可以交给一组特殊的超级用户。

接下来的几个步骤将使用Visual Studio Code IDE和 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 但可以使用任何工具和您配置的任何IDE来执行 **提取** 或 **导入** AEM本地实例中的内容。

1. 在Visual Studio Code IDE中，确保您具有 **VSCode AEM同步** 通过Marketplace扩展安装：

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 展开 **ui.content** 模块并导航到 `/conf/wknd-spa-react/settings/wcm/templates`.

3. **右键单击** 该 `templates` 文件夹并选择 **从AEM服务器导入**：

   ![Vscode导入模板](./assets/map-components/import-aem-servervscode.png)

4. 重复这些步骤以导入内容，但选择 **策略** 文件夹位于 `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect `filter.xml` 文件位于 `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   此 `filter.xml` 文件负责标识随包一起安装的节点的路径。 请注意 `mode="merge"` 对于每个表示现有内容将不会被修改的过滤器，将只添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署必须更新 **非** 覆盖内容。 请参阅 [FileVault文档](https://jackrabbit.apache.org/filevault/filter.html) 以了解有关使用筛选条件元素的更多详细信息。

   比较 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解每个模块管理的不同节点。

## （额外练习）创建自定义图像组件 {#bonus-image}

SPA图像组件已由React Core组件提供。 但是，如果您希望获得额外的练习，请创建自己的映射到AEM的React实施 [图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 此 `Image` 组件是 **内容** 组件。

### Inspect和JSON

在跳转到SPA代码之前，请检查AEM提供的JSON模型。

1. 导航至 [核心组件库中的图像示例](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![图像核心组件JSON](./assets/map-components/image-json.png)

   属性 `src`， `alt`、和 `title` 用于填充SPA `Image` 组件。

   >[!NOTE]
   >
   > 公开其他图像属性(`lazyEnabled`， `widths`)允许开发人员创建自适应和延迟加载组件。 本教程中构建的组件非常简单，可以 **非** 使用这些高级属性。

### 实施图像组件

1. 接下来，创建一个名为的新文件夹 `Image` 下 `ui.frontend/src/components`.
1. 在 `Image` 文件夹创建新文件，名为 `Image.js`.

   ![Image.js文件](./assets/map-components/image-js-file.png)

1. 添加以下内容 `import` 对帐单 `Image.js`：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. 然后添加 `ImageEditConfig` 要确定何时在AEM中显示占位符，请执行以下操作：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   占位符将显示 `src` 属性未设置。

1. 下次实施 `Image` class：

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

   上述代码将呈现 `<img>` 基于prop `src`， `alt`、和 `title` 由JSON模型传入。

1. 添加 `MapTo` 用于将React组件映射到AEM组件的代码：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   记下字符串 `wknd-spa-react/components/image` 与AEM组件在中的位置相对应 `ui.apps` 在： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. 创建新文件，名为 `Image.css` 在同一目录中添加以下内容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. 在 `Image.js` 在文件顶部下方添加对文件的引用 `import` 语句：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. 打开文件 `ui.frontend/src/components/import-components.js` 并添加对新的 `Image` 组件：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. 在 `import-components.js` 注释掉React核心组件图像：

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   这将确保改用我们的自定义图像组件。

1. 使用Maven将SPA代码从项目的根部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在AEM中Inspect SPA。 页面上的任何图像组件都应继续工作。 在渲染的输出中Inspect，您应该会看到自定义图像组件（而不是React核心组件）的标记。

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
