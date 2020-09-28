---
title: 扩展组件 | AEM SPA编辑器和React入门
description: 了解如何扩展现有核心组件以与AEM SPA Editor一起使用。 了解如何向现有组件添加属性和内容是扩展AEM SPA Editor实现功能的强大技术。 了解如何使用委托模式扩展Sling模型和Sling资源合并的功能。
sub-product: 站点
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '1969'
ht-degree: 1%

---


# 扩展核心组件 {#extend-component}

了解如何扩展现有核心组件以与AEM SPA Editor一起使用。 了解如何扩展现有组件是自定义和扩展AEM SPA Editor实现功能的强大技术。

## 目标

1. 使用其他属性和内容扩展现有核心组件。
2. 了解使用组件继承的基本内容 `sling:resourceSuperType`。
3. 了解如何利用 [Sling模型](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 的委托模式来重用现有逻辑和功能。

## 您将构建的内容

在本章中，将 `Card` 创建新组件。 该组 `Card` 件将扩展图 [像核心组件，添加其他内容字段](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html) （如标题和行动动员按钮），以对SPA中的其他内容执行Teaser的角色。

![卡组件的最终创作](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 在实际实施中，可能更合适的做法是简单地使用Teaser [组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) ，然后扩展 [图像核心组件以](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html) 根据项目要求制作 `Card` 组件。 始终建议尽可能直 [接使用核](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html) 心组件。

## 前提条件

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/extend-component-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统WKND参考站点安 [装完成的包](https://github.com/adobe/aem-guides-wknd/releases/latest)。 WKND参考站 [点提供的图像](https://github.com/adobe/aem-guides-wknd/releases/latest) ，将在WKND SPA上重新使用。 可以使用AEM Package Manager [安装包](http://localhost:4502/crx/packmgr/index.jsp)。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) ，或通过切换到分支在本地签出代码 `React/extend-component-solution`。

## Inspect初始卡实施

章节启动代码已提供初始卡组件。 Inspect是卡实施的起点。

1. 在您选择的IDE中，打开模 `ui.apps` 块。
2. 导航到 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card` 并视图 `.content.xml` 文件。

   ![卡组件AEM定义开始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   该属性 `sling:resourceSuperType` 指向 `wknd-spa-react/components/image` 表示该组 `Card` 件将继承WKND SPA图像组件的所有功能。

3. Inspect `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   注意， `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`。 这表示WKND SPA图像组件继承了核心组件图像的所有功能。

   也称为代理模 [式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling资源继承是一种功能强大的设计模式，允许子组件在需要时继承功能和扩展／覆盖行为。 Sling继承支持多级继承，因此新组件最终 `Card` 会继承核心组件图像的功能。

   许多开发团队都努力成为DRY（不要重复自己）。 Sling继承使AEM成为可能。

4. 在文件 `card` 夹下，打开文 `_cq_dialog/.content.xml`件。

   此文件是组件的组件对话框定 `Card` 义。 如果使用Sling继承，则可能使用Sling资源合并 [的功能](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) ，覆盖或扩展对话框的部分。 在此示例中，新选项卡已添加到对话框中，以从作者处捕获其他数据以填充卡组件。

   诸如属性 `sling:orderBefore` 之类的属性允许开发人员选择插入新选项卡或表单字段的位置。 在这种情况下， `Text` 选项卡将插入到选项卡 `asset` 之前。 要充分利用Sling资源合并，了解图像组件对话框的原始对话框节 [点结构很重要](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)。

5. 在文件 `card` 夹下，打开文 `_cq_editConfig.xml`件。 此文件指示AEM创作UI中的拖放行为。 扩展图像组件时，资源类型与组件本身匹配很重要。 查看节 `<parameters>` 点：

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大多数组件都不需要， `cq:editConfig`图像组件的图像和子代后代都是例外。

6. 在IDE切换到模块 `ui.frontend` 中，导航到 `ui.frontend/src/components/Card`:

   ![反应组件开始](assets/extend-component/react-card-component-start.png)

7. Inspect `Card.js`。

   该组件已被断开，可使用标准函 `Card` 数映射到AEM `MapTo` 组件。

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect `get imageContent()`:

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   在此示例中，我们选择只通过传递组件中的图像来 `Image` 重新使用现 `this.props` 有的React Image `Card` 组件。 在本教程的后 `get bodyContent()` 面，将实现该方法以显示标题、日期和行动动员按钮。

## 更新模板策略

通过此初 `Card` 始实施查看AEM SPA Editor中的功能。 要查看初始组 `Card` 件，需要更新模板策略。

1. 将起始代码部署到AEM的本地实例（如果您尚未部署）:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 导航到SPA页面模板(位 [于http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html))。
3. 更新布局容器的策略，将新组件添 `Card` 加为允许的组件：

   ![更新布局容器策略](assets/extend-component/card-component-allowed.png)

   保存对策略所做的更改，并将该组 `Card` 件作为允许的组件进行观察：

   ![卡组件作为允许的组件](assets/extend-component/card-component-allowed-layout-container.png)

## 创作初始卡组件

接下来，使 `Card` 用AEM SPA编辑器创作组件。

1. 导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
2. 在模 `Edit` 式中，将 `Card` 组件添加到 `Layout Container`:

   ![插入新组件](assets/extend-component/insert-card-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![添加图像](assets/extend-component/card-add-image.png)

4. 打开组 `Card` 件对话框，注意添加了“文本” **选项卡** 。
5. 在“文本”选项卡上输 **入以** 下值：

   ![文本组件选项卡](assets/extend-component/card-component-text.png)

   **卡路径** -在SPA主页下选择页面。

   **CTA文本** -“阅读更多”

   **卡片标题** -留空

   **从链接页面获取标题** -选中复选框以指示为true。

6. 更新资产 **元数据** 选项卡，为替代文 **本和题注添** 加 **值**。

   当前，更新对话框后不显示其他更改。 要向React Component公开新字段，我们需要更新该组件的Sling `Card` Model。

7. 打开新选项卡并导 [航到CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card)。 Inspect下方的内容 `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid` 节点以查找 `Card` 组件内容。

   ![CRXDE-Lite组件属性](assets/extend-component/crxde-lite-properties.png)

   请注意， `cardPath`对话框 `ctaText`会 `titleFromPage` 保留属性。

## 更新卡Sling型号

要最终将组件对话框中的值暴露给React组件，我们需要更新为组件填充JSON的Sling `Card` 模型。 我们还有机会实施两个业务逻辑：

* 如果 `titleFromPage` 为 **true**，则返回指定页面的标题，否则 `cardPath` 返回文本字段 `cardTitle` 的值。
* 返回指定页面的上次修改日期 `cardPath`。

返回您选择的IDE并打开模 `core` 块。

1. 在打开文 `Card.java` 件 `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`。

   请注意， `Card` 该接口当 `com.adobe.cq.wcm.core.components.models.Image` 前已扩展，因此继承了该接口的所有 `Image` 方法。 界 `Image` 面已扩展界 `ComponentExporter` 面，该界面允许Sling Model导出为JSON并由SPA编辑器映射。 因此，我们无需像在“自定义 `ComponentExporter` 组件”一章中那样显 [式扩展界面](custom-component.md)。

2. 向接口添加以下方法：

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   这些方法将通过JSON模型API公开，并传递给React组件。

3. 打开 `CardImpl.java`. 这是接口的 `Card.java` 实现。 为加快教程，已部分停止了此实现。  请注意使用和 `@Model` 注 `@Exporter` 释以确保Sling Model能够通过Sling Model Exporter序列化为JSON。

   `CardImpl.java` 还使用Sling [模型的委托模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) ，以避免从图像核心组件重写所有逻辑。

4. 请遵循以下规则：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述注释将实例化一个基于组 `image` 件继承名 `sling:resourceSuperType` 称的图像 `Card` 对象。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然后，可以简单地使用该对 `image` 象来实现由接口定义的方 `Image` 法，而无需自己编写逻辑。 此技术用于 `getSrc()`和 `getAlt()` /或 `getTitle()`。

5. 接下来，实 `initModel()` 现方法，以根据 `cardPage` 值启动专用变量 `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   初始 `@PostConstruct initModel()` 化Sling模型时，将始终调用该模型，因此，这是初始化模型中其他方法可能使用的对象的良机。 它是 `pageManager` 通过注释为Sling Models提 [供的众多支持Java的全局对象之](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects)`@ScriptVariable` 一。 getPage [方法在路径中](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) ，并返回AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) 对象；如果路径未指向有效页面，则返回null。

   这将初始化变 `cardPage` 量，其他新方法将使用该变量返回有关基础链接页面的数据。

6. 查看已映射到已保存创作对话框的JCR属性的全局变量。 注 `@ValueMapValue` 释用于自动执行映射。

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   这些变量将用于实现接口的附加方 `Card.java` 法。

7. 实现接口中定义的其他 `Card.java` 方法：

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > 您可以在此处 [视图完成的CardImpl.java](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java)。

8. 打开终端窗口，使用目录中的Maven `core` 用户档案仅部署模 `autoInstallBundle` 块的更 `core` 新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案。

9. 视图JSON模型响应： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) and search for the `wknd-spa-react/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-react/us/en/home/page-1.html",
       ":type": "wknd-spa-react/components/card"
   }
   ```

   请注意，在Sling Model中更新方法后，JSON模型会使用其他键／值 `CardImpl` 对进行更新。

## 更新React组件

现在，JSON模型中填充了新属 `ctaLinkURL`性 `ctaText`、 `cardTitle` 并且 `cardLastModified` 我们可以更新React组件来显示这些属性。

1. 返回IDE并打开模 `ui.frontend` 块。 或者，从新的终端窗口开始webpack dev服务器以实时查看更改：

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 打开 `Card.js` 位置 `ui.frontend/src/components/Card/Card.js`。
3. 添加用于 `get ctaButton()` 呈现行动调用的方法：

   ```js
   import {Link} from "react-router-dom";
   ...
   
   export default class Card extends Component {
   
       get ctaButton() {
           if(this.props && this.props.ctaLinkURL && this.props.ctaText) {
               return (
                   <div className="Card__action-container">
                       <Link to={this.props.ctaLinkURL} title={this.props.title}
                           className="Card__action-link">
                           {this.props.ctaText}
                       </Link>
                   </div>
               );
           }
   
           return null;
       }
       ...
   }
   ```

4. 添加一种方法， `get lastModifiedDisplayDate()` 以将日 `this.props.cardLastModified` 期转换为表示日期的本地化字符串。

   ```js
   export default class Card extends Component {
       ...
       get lastModifiedDisplayDate() {
           const lastModifiedDate = this.props.cardLastModified ? new Date(this.props.cardLastModified) : null;
   
           if (lastModifiedDate) {
               return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

5. 更新以 `get bodyContent()` 显示 `this.props.cardTitle` 并使用在以前步骤中创建的方法：

   ```js
   export default class Card extends Component {
       ...
       get bodyContent() {
          return (<div class="Card__content">
                       <h2 class="Card__title"> {this.props.cardTitle}
                           <span class="Card__lastmod">
                               {this.lastModifiedDisplayDate}
                           </span>
                       </h2>
                       {this.ctaButton}
               </div>);
       }
       ...
   }
   ```

6. 已在添加了标题样式 `Card.scss` 、行动动员和上次修改日期的Sass规则。 通过在文件顶部添加以 `Card.js` 下行来包含这些样式：

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > 您可以在此处视图完 [成的React卡组件代码](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js)。

7. 使用Maven从项目的根部部署对AEM的完整更改：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. 导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) ，以查看更新的组件：

   ![更新了AEM中的卡组件](assets/extend-component/updated-card-in-aem.png)

9. 您应该能够重新创作现有内容以创建类似于以下内容的页面：

   ![卡组件的最终创作](assets/extend-component/final-authoring-card.png)

## 恭喜！ {#congratulations}

恭喜您，您学习了如何使用AEM组件扩展，以及Sling模型和对话框如何与JSON模型配合使用。

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) ，或通过切换到分支在本地签出代码 `React/extend-component-solution`。
