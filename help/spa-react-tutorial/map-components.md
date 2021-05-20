---
title: 将SPA组件映射到AEM组件 | AEM SPA Editor和React快速入门
description: 了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
sub-product: 站点
feature: SPA编辑器
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 1%

---


# 将SPA组件映射到AEM组件{#map-components}

了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。

本章更深入地介绍了AEM JSON模型API，以及如何将AEM组件公开的JSON内容自动作为prop注入到React组件中。

## 目标

1. 了解如何将AEM组件映射到SPA组件。
2. 了解&#x200B;**Container**&#x200B;组件与&#x200B;**Content**&#x200B;组件之间的差异。
3. 创建新的React组件，以映射到现有AEM组件。

## 将构建的内容

本章将检查提供的`Text` SPA组件如何映射到AEM `Text`组件。 将创建一个新的`Image` SPA组件，该组件可在SPA中使用并在AEM中创作。 **布局容器**&#x200B;和&#x200B;**模板编辑器**&#x200B;策略的开箱即用功能还将用于创建外观稍有变化的视图。

![章节示例最终创作](./assets/map-components/final-page.png)

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

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

   如果使用[AEM 6.x](overview.md#compatibility)添加`classic`配置文件：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上查看完成的代码，或通过切换到分支`React/map-components-solution`在本地签出代码。

## 映射方法

基本概念是将SPA组件映射到AEM组件。 AEM组件、运行服务器端，将内容导出为JSON模型API的一部分。 SPA会使用JSON内容，并在浏览器中运行客户端。 将创建SPA组件与AEM组件之间的1:1映射。

![将AEM组件映射到React组件的概述](./assets/map-components/high-level-approach.png)

*将AEM组件映射到React组件的概述*

## Inspect文本组件

[AEM项目原型](https://github.com/adobe/aem-project-archetype)提供了映射到AEM [文本组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)的`Text`组件。 这是&#x200B;**content**&#x200B;组件的示例，其中组件从AEM中呈现&#x200B;*content*。

让我们看看组件的工作方式。

### Inspect JSON模型

1. 在跳转到SPA代码之前，请务必了解AEM提供的JSON模型。 导航到[核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)并查看文本组件的页面。 核心组件库提供了所有AEM核心组件的示例。
2. 选择&#x200B;**JSON**&#x200B;选项卡，以获取以下示例之一：

   ![文本JSON模型](./assets/map-components/text-json.png)

   您应会看到三个资产：`text`、`richText`和`:type`。

   `:type` 是一个保留属性，其中列 `sling:resourceType` 出了AEM组件的（或路径）。值`:type`用于将AEM组件映射到SPA组件。

   `text` 和是 `richText` 将公开给SPA组件的其他属性。

### Inspect文本组件

1. 打开新终端，然后导航到项目内的`ui.frontend`文件夹。 运行`npm install`，然后运行`npm start`以启动&#x200B;**webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   `ui.frontend`模块当前设置为使用[模拟JSON模型](./integrate-spa.md#mock-json)。

2. 您应会看到一个新的浏览器窗口，该窗口将打开到[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![包含模拟内容的WebPack开发服务器](./assets/map-components/initial-start.png)

3. 在选择的IDE中，打开WKND SPA的AEM项目。 展开`ui.frontend`模块并在`ui.frontend/src/components/Text/Text.js`下打开文件`Text.js`:

   ![Text.js React组件源代码](./assets/map-components/vscode-ide-text-js.png)

4. 我们将检查的第一个区域是位于~40行的`class Text`:

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

   `Text` 是标准React组件。组件使用`this.props.richText`确定要渲染的内容是富文本还是纯文本。 使用的实际“content”来自`this.props.text`。 为避免潜在的XSS攻击，富文本在使用[alisalSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)渲染内容之前会通过`DOMPurify`进行转义。 在练习前面的JSON模型中，回顾`richText`和`text`属性。

5. 接下来，查看位于~29行的`TextEditConfig`:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   上述代码负责确定何时在AEM创作环境中渲染占位符。 如果`isEmpty`方法返回&#x200B;**true**，则将呈现占位符。

6. 最后，查看~line 62处的`MapTo`调用：

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` 由AEM SPA Editor JS SDK(`@adobe/aem-react-editable-components`)提供。路径`wknd-spa-react/components/text`表示AEM组件的`sling:resourceType`。 此路径与之前观察到的JSON模型公开的`:type`相匹配。 `MapTo` 负责解析JSON模型响应并将正确的值传递 `props` 到SPA组件。

   您可以在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`中找到AEM `Text`组件定义。

7. 通过修改位于`ui.frontend/public/mock-content/mock.model.json`的`mock.model.json`文件进行实验。 在第62行，更新第一个`Text`值以使用&#x200B;**`H1`**&#x200B;和&#x200B;**`u`**&#x200B;标记：

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   导航到[http://localhost:3000](http://localhost:3000)以查看效果：

   ![更新了文本模型](./assets/map-components/updated-text-model.png)

   尝试在&#x200B;**true** / **false**&#x200B;之间切换`richText`属性，以查看正在执行的渲染逻辑。

8. Inspect `Text.scss` `ui.frontend/src/components/Text/Text.scss`。

   此文件由本章的起始代码库添加，并利用上一章中添加的[Sass](https://sass-lang.com/)功能。 请注意`ui.frontend/src/styles/_variables.scss`中引用的变量。

## 创建图像组件

接下来，创建一个映射到AEM [图像组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html)的`Image` React组件。 `Image`组件是&#x200B;**content**&#x200B;组件的另一个示例。

### Inspect JSON

在跳转到SPA代码之前，请检查AEM提供的JSON模型。

1. 导航到核心组件库](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)中的[图像示例。

   ![图像核心组件JSON](./assets/map-components/image-json.png)

   将使用`src`、`alt`和`title`的属性填充SPA `Image`组件。

   >[!NOTE]
   >
   > 还公开了其他图像属性(`lazyEnabled`、`widths`)，允许开发人员创建自适应和延迟加载组件。 本教程中构建的组件将会很简单，**不会**&#x200B;使用这些高级属性。

2. 返回到IDE并在`ui.frontend/public/mock-content/mock.model.json`处打开`mock.model.json`。 由于这是我们项目的新组件，因此我们需要“模拟”图像JSON。

   在~70行，为`image`模型添加JSON条目（不要忘记尾随的逗号`,`在第二个`text_23828680`之后），并更新`:itemsOrder`数组。

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

   该项目包含`/mock-content/adobestock-140634652.jpeg`的示例图像，该示例图像将与&#x200B;**webpack-dev-server**&#x200B;一起使用。

   您可以在此处](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)查看完整的[mock.model.json。

### 实施图像组件

1. 接下来，在`ui.frontend/src/components`下创建一个名为`Image`的新文件夹。
2. 在`Image`文件夹下，创建一个名为`Image.js`的新文件。

   ![Image.js文件](./assets/map-components/image-js-file.png)

3. 将以下`import`语句添加到`Image.js`中：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. 然后添加`ImageEditConfig`以确定何时在AEM中显示占位符：

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   如果未设置`src`属性，则将显示占位符。

5. 接下来实现`Image`类：

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

   上述代码将根据JSON模型传入的prop `src`、`alt`和`title`来呈现`<img>`。

6. 添加`MapTo`代码以将React组件映射到AEM组件：

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   请注意，字符串`wknd-spa-react/components/image`对应于AEM组件在`ui.apps`中的位置：`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`。

7. 在同一目录中创建名为`Image.scss`的新文件，并添加以下内容：

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. 在`Image.js`中，在`import`语句下方的顶部添加对文件的引用：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   您可以在此处](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)查看已完成的[Image.js。

9. 打开文件`ui.frontend/src/components/import-components.js`并添加对新`Image`组件的引用：

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. 如果尚未启动，请启动&#x200B;**webpack-dev-server**。 导航到[http://localhost:3000](http://localhost:3000)，此时您应会看到图像渲染器：

   ![模拟中添加了图像](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **奖金挑战**:在中实施新方 `Image.js` 法，以在图 `this.props.title` 像下方显示值作为标题。

## 在AEM中更新策略

`Image`组件仅在&#x200B;**webpack-dev-server**&#x200B;中可见。 接下来，将更新的SPA部署到AEM并更新模板策略。

1. 停止&#x200B;**webpack-dev-server**&#x200B;并从项目的根目录中，使用您的Maven技能将更改部署到AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 从“AEM开始”屏幕导航到&#x200B;**工具** > **模板** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**。

   选择并编辑&#x200B;**SPA页面**:

   ![编辑SPA页面模板](./assets/map-components/edit-spa-page-template.png)

3. 选择&#x200B;**布局容器**&#x200B;并单击其&#x200B;**policy**&#x200B;图标以编辑策略：

   ![布局容器策略](./assets/map-components/layout-container-policy.png)

4. 在&#x200B;**允许的组件** > **WKND SPA React - Content**&#x200B;下，检查&#x200B;**Image**&#x200B;组件：

   ![已选择图像组件](./assets/map-components/check-image-component.png)

   在&#x200B;**默认组件** > **添加映射**&#x200B;下，选择&#x200B;**图像 — WKND SPA React — 内容**&#x200B;组件：

   ![设置默认组件](./assets/map-components/default-components.png)

   输入&#x200B;**mime类型**，共`image/*`。

   单击&#x200B;**完成**&#x200B;以保存策略更新。

5. 在&#x200B;**布局容器**&#x200B;中，单击&#x200B;**Text**&#x200B;组件的&#x200B;**policy**&#x200B;图标：

   ![文本组件策略图标](./assets/map-components/edit-text-policy.png)

   创建名为&#x200B;**WKND SPA Text**&#x200B;的新策略。 在&#x200B;**Plugins** > **Formatting** >下，选中所有框以启用其他格式选项：

   ![启用RTE格式](assets/map-components/enable-formatting-rte.png)

   在&#x200B;**Plugins** > **段落样式** >下，选中复选框以&#x200B;**启用段落样式**:

   ![启用段落样式](./assets/map-components/text-policy-enable-paragraphstyles.png)

   单击&#x200B;**完成**&#x200B;以保存策略更新。

6. 导航到&#x200B;**Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。

   您还应该能够编辑`Text`组件，并在&#x200B;**全屏**&#x200B;模式下添加其他段落样式。

   ![全屏富文本编辑](assets/map-components/full-screen-rte.png)

7. 您还应该能够从&#x200B;**资产查找器**&#x200B;中拖放图像：

   ![拖放图像](./assets/map-components/drag-drop-image.gif)

8. 通过[AEM Assets](http://localhost:4502/assets.html/content/dam)添加您自己的映像，或为标准[WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest)安装完成的代码库。 [WKND引用站点](https://github.com/adobe/aem-guides-wknd/releases/latest)包含许多可在WKND SPA上重新使用的图像。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安装包。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect布局容器

AEM SPA Editor SDK会自动提供对&#x200B;**布局容器**&#x200B;的支持。 **布局容器**（如名称所示）是&#x200B;**容器**&#x200B;组件。 容器组件是接受表示&#x200B;*其他*&#x200B;组件并动态实例化它们的JSON结构的组件。

让我们进一步检查布局容器。

1. 在浏览器中，导航到[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON模型API — 响应式网格](./assets/map-components/responsive-grid-modeljson.png)

   **布局容器**&#x200B;组件的`sling:resourceType`为`wcm/foundation/components/responsivegrid`，并且可由SPA编辑器使用`:type`属性进行识别，与`Text`和`Image`组件类似。

   SPA编辑器中提供了使用[布局模式](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)重新调整组件大小的相同功能。

2. 返回到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 添加其他&#x200B;**Image**&#x200B;组件，然后尝试使用&#x200B;**布局**&#x200B;选项重新调整组件大小：

   ![使用布局模式重新调整图像大小](./assets/map-components/responsive-grid-layout-change.gif)

3. 重新打开JSON模型[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，并在JSON中观察`columnClassNames` :

   ![列类名称](./assets/map-components/responsive-grid-classnames.png)

   类名称`aem-GridColumn--default--4`表示组件应具有4列宽，基于12列网格。 有关[响应式网格的更多详细信息，请参阅此处](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)。

4. 返回到IDE，在`ui.apps`模块中，有一个在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`定义的客户端库。 打开文件`less/grid.less`。

   此文件确定&#x200B;**布局容器**&#x200B;使用的断点（`default`、`tablet`和`phone`）。 此文件将根据项目规范进行自定义。 当前，断点设置为`1200px`和`650px`。

5. 您应该能够使用`Text`组件的响应功能和更新的富文本策略来创作如下视图：

   ![章节示例最终创作](./assets/map-components/final-page.png)

## 恭喜！ {#congratulations}

恭喜，您已学习如何将SPA组件映射到AEM组件，并且已实施新的`Image`组件。 您还有机会探索&#x200B;**布局容器**&#x200B;的响应功能。

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution)上查看完成的代码，或通过切换到分支`React/map-components-solution`在本地签出代码。

### 后续步骤{#next-steps}

[导航和路由](navigation-routing.md)  — 通过SPA Editor SDK将映射到AEM页面，了解如何支持SPA中的多个视图。动态导航是使用React Router实现的，并添加到现有的Header组件中。

## 附加练习 — 保留用于源代码控制的配置 {#bonus}

在很多情况下，特别是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源控制中非常有价值。 这可确保所有开发人员针对同一组内容和配置开展工作，并可确保各环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的做法就可以交给一组特定的高级用户。

接下来的几个步骤将使用Visual Studio代码IDE和[VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)执行，但可能会使用任何工具和任何已配置为&#x200B;**pull**&#x200B;或&#x200B;**从AEM的本地实例导入**&#x200B;内容的IDE来执行。

1. 在Visual Studio代码IDE中，确保已通过Marketplace扩展安装了&#x200B;**VSCode AEM Sync**:

   ![VSCode AEM同步](./assets/map-components/vscode-aem-sync.png)

2. 在项目资源管理器中展开&#x200B;**ui.content**&#x200B;模块，然后导航到`/conf/wknd-spa-react/settings/wcm/templates`。

3. **右键+单** 击文 `templates` 件夹，然后选 **择从AEM Server导入**:

   ![VSCode导入模板](./assets/map-components/import-aem-servervscode.png)

4. 重复这些步骤以导入内容，但选择位于`/conf/wknd-spa-react/settings/wcm/templates/policies`的&#x200B;**policys**&#x200B;文件夹。

5. Inspect位于`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`文件。

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

   `filter.xml`文件负责标识将随包一起安装的节点的路径。 请注意每个过滤器上的`mode="merge"`，指示不会修改现有内容，只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署必须&#x200B;**不**&#x200B;覆盖内容。 有关使用过滤器元素的更多详细信息，请参阅[FileVault文档](https://jackrabbit.apache.org/filevault/filter.html)。

   比较`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每个模块管理的不同节点。
