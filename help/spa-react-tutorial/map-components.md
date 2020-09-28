---
title: 将SPA组件映射到AEM组件 | AEM SPA编辑器和React入门
description: 了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射使用户能够在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
sub-product: 站点
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 1%

---


# 将SPA组件映射到AEM组件 {#map-components}

了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射使用户能够在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。

本章将更深入地介绍AEM JSON模型API，以及AEM组件公开的JSON内容如何作为prop自动注入到React组件中。

## 目标

1. 了解如何将AEM组件映射到SPA组件。
2. 了解容器组 **件** 和内容 **组件** 。
3. 新建一个映射到现有AEM组件的React组件。

## 您将构建的内容

本章将检查提供的SPA `Text` 组件如何映射到AEM `Text`组件。 将创 `Image` 建可在SPA中使用并在AEM中创作的新SPA组件。 布局容器和模 **板编辑器****策略的开箱** ，还将用于创建外观更为不同的视图。

![章节范例最终创作](./assets/map-components/final-page.png)

## 前提条件

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) ，或通过切换到分支在本地签出代码 `React/map-components-solution`。

## 映射方法

基本概念是将SPA组件映射到AEM组件。 AEM组件、运行服务器端、将内容作为JSON模型API的一部分导出。 JSON内容由SPA使用，在浏览器中运行客户端。 将创建SPA组件与AEM组件之间的1:1映射。

![将AEM组件映射到React Component的高级概述](./assets/map-components/high-level-approach.png)

*将AEM组件映射到React Component的高级概述*

## Inspect文本组件

AEM [Project](https://github.com/adobe/aem-project-archetype) Archetype提供 `Text` 映射到AEM Text组件的 [组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)。 这是内容组件的 **示例** ，其中它从AEM *呈现内容* 。

让我们看看组件的工作方式。

### InspectJSON模型

1. 在跳入SPA代码之前，了解AEM提供的JSON模型非常重要。 导航到核 [心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) ，然后视图文本组件的页面。 核心组件库提供所有AEM核心组件的示例。
2. 选择 **JSON** 选项卡，用于其中一个示例：

   ![文本JSON模型](./assets/map-components/text-json.png)

   您应当看到三个属性： `text`、 `richText`和 `:type`。

   `:type` 是保留属性，它列表 `sling:resourceType` AEM组件的（或路径）。 值是用 `:type` 于将AEM组件映射到SPA组件的值。

   `text` 以 `richText` 及SPA组件中的其他属性。

### Inspect文本组件

1. 打开新终端并导航到项 `ui.frontend` 目内的文件夹。 运 `npm install` 行，然 `npm start` 后开始 **webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   该 `ui.frontend` 模块当前已设置为使用模 [拟JSON模型](./integrate-spa.md#mock-json)。

2. 您应当看到一个新的浏览器窗口打开 [到http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![具有模拟内容的Webpack开发服务器](./assets/map-components/initial-start.png)

3. 在您选择的IDE中，为WKND SPA打开AEM项目。 展开模 `ui.frontend` 块并在以下位置打 `Text.js` 开文件 `ui.frontend/src/components/Text/Text.js`:

   ![Text.js React组件源代码](./assets/map-components/vscode-ide-text-js.png)

4. 我们将检查的第一个区域是 `class Text` 第40行：

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

   `Text` 是标准的React组件。 组件用 `this.props.richText` 于确定要渲染的内容是富文本还是纯文本。 实际使用的“内容”来自 `this.props.text`。 为避免潜在的XSS攻击，在使用危险的SetInnerHTML `DOMPurify` 渲染内 [容之前](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) ，会先通过转义富文本。 在练习 `richText` 的 `text` 前面，从JSON模型中调用该属性和属性。

5. 接下来，请看 `TextEditConfig` ~29号线：

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   以上代码负责确定何时在AEM作者环境中呈现占位符。 如果方 `isEmpty` 法返 **回true** ，则占位符将呈现。

6. 最后看看~62 `MapTo` 号线的通话：

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 由AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)提供。 路径 `wknd-spa-react/components/text` 表示AEM `sling:resourceType` 组件的属性。 此路径与先前观察 `:type` 的JSON模型所显示的路径匹配。 `MapTo` 负责解析JSON模型响应并将正确值传 `props` 递到SPA组件。

   可在上找到AEM `Text` 组件定 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`义。

7. 通过在修改文 `mock.model.json` 件进行试 `ui.frontend/public/mock-content/mock.model.json`验。 在~第62行更新第 `Text` 一个值以使用 **`H1`** 和标 **`u`** 记：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   导航到 [http://localhost:3000](http://localhost:3000) ，查看效果：

   ![更新的文本模型](./assets/map-components/updated-text-model.png)

   尝试在true `richText` / false **之间切** 换属 **性，** 以查看实际的渲染逻辑。

8. Inspect `Text.scss` 在 `ui.frontend/src/components/Text/Text.scss`。

   此文件由本章的起始代码库添加，并利用在上 [一章中](https://sass-lang.com/) 添加的Sass功能。 请注意引用的变量 `ui.frontend/src/styles/_variables.scss`。

## 创建图像组件

然后，创建映 `Image` 射到AEM Image组件的React [组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html)。 组 `Image` 件是内容组件的另 **一示例** 。

### InspectJSON

跳入SPA代码前，请检查AEM提供的JSON模型。

1. 导航到核 [心组件库中的图像示例](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)。

   ![图像核心组件JSON](./assets/map-components/image-json.png)

   和的 `src`属 `alt`性将 `title` 用于填充SPA组 `Image` 件。

   >[!NOTE]
   >
   > 还有其他公开(`lazyEnabled`、 `widths`)的图像属性允许开发人员创建自适应和延迟加载组件。 本教程中构建的组件将很简单，不会 **使用** 这些高级属性。

2. 返回IDE并打开 `mock.model.json` 位 `ui.frontend/public/mock-content/mock.model.json`置 由于这是我们项目的新组件，因此我们需要“模拟”图像JSON。

   在~第70行为模型添加一个JSON `image` 条目(不要忘记第二个后面的 `,` 逗号) `text_23828680`并更新数 `:itemsOrder` 组。

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   项目包含将与webpack-dev- `/mock-content/adobestock-140634652.jpeg` server一起使用的 **示例图像**。

   您可以在此处 [视图完整mock.model.json](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)。

### 实现图像组件

1. 然后，创建名为“”的新 `Image` 文件夹 `ui.frontend/src/components`。
2. 在文件 `Image` 夹下面创建一个名为的新文 `Image.js`件。

   ![Image.js文件](./assets/map-components/image-js-file.png)

3. 将以下语句 `import` 添加到 `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 然后添加 `ImageEditConfig` 以确定何时在AEM中显示占位符：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未设置属性， `src` 则将显示占位符。

5. 接下来实 `Image` 现类：

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

   上述代码将根据 `<img>` JSON模型传 `src`入 `alt`的prop `title` 进行呈现。

6. 添加将 `MapTo` React组件映射到AEM组件的代码：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   请注意， `wknd-spa-react/components/image` 字符串与AEM组件在以下位置 `ui.apps` 对应： `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. 在同一目录中创 `Image.scss` 建名为的新文件，并添加以下内容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. 在 `Image.js` 语句下方顶部添加对文件的引 `import` 用：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   您可以在此处视图 [完成的Image.js](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)。

9. 打开文件 `ui.frontend/src/components/import-components.js` 并添加对新组件的引 `Image` 用：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. 如果尚未启动，请开始 **webpack-dev-server**。 导航到 [http://localhost:3000](http://localhost:3000) ，您应当看到图像渲染：

   ![已添加到模型的图像](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **奖金挑战**:在中实现新方 `Image.js` 法，以在图像下 `this.props.title` 面将值显示为字幕。

## 更新AEM中的策略

组 `Image` 件仅在webpack- **dev-server中可见**。 接下来，将更新的SPA部署到AEM并更新模板策略。

1. 停止 **webpack-dev-server** ，从项目的根目录，使用您的Maven技能将更改部署到AEM::

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 从AEM开始屏幕导航到 **工具** > **模板** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**。

   选择并编辑 **SPA页面**:

   ![编辑SPA页面模板](./assets/map-components/edit-spa-page-template.png)

3. 选择布 **局容器** ，然后单击其 **策略图** 标以编辑策略：

   ![布局容器策略](./assets/map-components/layout-container-policy.png)

4. 在“允 **许的组件** ” > **WKND SPA React - Content** > check the **Image** component:

   ![已选择图像组件](./assets/map-components/check-image-component.png)

   在“ **默认组** 件” > **“添加映射** ”下，选 **择“图像- WKND SPA React —— 内容组件** ”:

   ![设置默认组件](./assets/map-components/default-components.png)

   输入 **MIME类型**`image/*`。

   单击 **完成** ，以保存策略更新。

5. 在“布 **局”容器** ，单 **击“文本** ”组件的 **策略图** 标：

   ![文本组件策略图标](./assets/map-components/edit-text-policy.png)

   创建名为WKND SPA Text **的新策略**。 在“插 **件** ”>“格 **式设置** ”>下，选中所有框以启用其他格式设置选项：

   ![启用RTE格式](assets/map-components/enable-formatting-rte.png)

   在“插 **件** ”>“段 **落样式** ”>下选中复选框 **以启用段落样式**:

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   单击 **完成** ，以保存策略更新。

6. 导航到主 **页**[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

   您还应能够在全屏模 `Text` 式下编辑组件和添加其 **他段落样式** 。

   ![全屏富文本编辑](assets/map-components/full-screen-rte.png)

7. 您还应能够从资产查找器中拖放图 **像**:

   ![拖放图像](./assets/map-components/drag-drop-image.gif)

8. 通过AEM Assets添 [加您](http://localhost:4502/assets.html/content/dam) 自己的图像 [，或为标准WKND参考站点安装完](https://github.com/adobe/aem-guides-wknd/releases/latest)成的代码库。 WKND [参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) (WKND SPA)包含许多可在WKND SPA上重新使用的图像。 可以使用AEM Package Manager [安装包](http://localhost:4502/crx/packmgr/index.jsp)。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect版面容器

AEM SPA Editor **SDK自动** 提供对布局容器的支持。 布 **局容器**（由名称指示）是一个 **容器组件** 。 容器组件是接受JSON结构的组件，它们表示其 *他组件* ，并动态实例化它们。

让我们进一步检查布局容器。

1. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API —— 响应式网格](./assets/map-components/responsive-grid-modeljson.png)

   布 **局容器** (Layout `sling:resourceType``wcm/foundation/components/responsivegrid` )组件具有一个属性，并由SPA Editor使用该属性进行识别， `:type` 就像和组件一 `Text` 样 `Image` 。

   SPA编辑器也提供使用布局模式 [重新调整组](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) 件大小的相同功能。

2. 返回 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 添加其 **他图像** 组件，并尝试使用“布局”选项重新调整 **它们** 大小：

   ![使用布局模式重新调整图像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新打开JSON模 [型](http://localhost:4502/content/wknd-spa-react/us/en.model.json) http://localhost:4502/content/wknd-spa-react/us/en.model.json，并 `columnClassNames` 观察JSON的一部分：

   ![列类名称](./assets/map-components/responsive-grid-classnames.png)

   类名表 `aem-GridColumn--default--4` 示组件应基于12列网格为4列宽。 有关响应式网 [格的更多详细信息，请参阅](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回IDE，在模块 `ui.apps` 中定义一个客户端库 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`。 Open the file `less/grid.less`.

   此文件决定布局`default`容器 `tablet`使用 `phone`的断 **点（、和）**。 此文件旨在根据项目规范进行自定义。 当前断点设置为 `1200px` 和 `650px`。

5. 您应该能够使用组件的响应功能和更新的富文本策 `Text` 略来创作视图，如下所示：

   ![章节范例最终创作](./assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜您，您学习了如何将SPA组件映射到AEM组件，并实施了一个新 `Image` 组件。 您还有机会探索布局容器的响应 **功能**。

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) ，或通过切换到分支在本地签出代码 `React/map-components-solution`。

### 后续步骤 {#next-steps}

[导航和路由](navigation-routing.md) -了解如何使用SPA编辑器SDK映射到AEM页面，从而支持SPA中的多个视图。 动态导航是使用React Router实现的，并添加到现有的Header组件中。

## 额外功能——保留源代码控制的配置 {#bonus}

在许多情况下，尤其是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源代码控制非常有价值。 这可确保所有开发人员针对同一组内容和配置工作，并确保环境之间的更多一致性。 一旦一个项目达到一定的成熟度，管理模板的做法就可以交给一组特殊的高级用户。

接下来的几个步骤将使用Visual Studio代码IDE和 [VSCode AEM](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) Sync进行，但可能使用您配置为从AEM的本地实例中拉取或导入内容的 **任何工****** 具和任何IDE进行。

1. 在Visual Studio代码IDE中，确保已通过Marketplace扩 **展安装VSCode** AEM Sync:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. 在项目 **资源管理器中展开** ui.content模块，然后导航到 `/conf/wknd-spa-react/settings/wcm/templates`。

3. **右键单击文件夹** ，然 `templates` 后选择“从AEM **Server导入”**:

   ![VSCode导入模板](./assets/map-components/import-aem-servervscode.png)

4. 重复这些步骤以导入内容，但选择位 **于** 的策略文件夹 `/conf/wknd-spa-react/settings/wcm/templates/policies`。

5. Inspect, `filter.xml` 文件位于 `ui.content/src/main/content/META-INF/vault/filter.xml`。

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

   文 `filter.xml` 件负责标识随软件包一起安装的节点的路径。 注意 `mode="merge"` 每个过滤器上的指示现有内容不会被修改，只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署不会覆盖 **内容** ，这一点很重要。 有关使用 [筛选器元素的](https://jackrabbit.apache.org/filevault/filter.html) 更多详细信息，请参阅FileVault文档。

   比较 `ui.content/src/main/content/META-INF/vault/filter.xml` 并 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解每个模块管理的不同节点。
