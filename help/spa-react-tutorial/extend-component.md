---
title: 扩展组件 | AEM SPA Editor和React入门
description: 了解如何扩展要与AEM SPA Editor一起使用的现有核心组件。 了解如何向现有组件添加属性和内容是扩展AEM SPA Editor实现功能的强大技术。 了解如何使用委托模式扩展Sling模型和Sling资源合并的功能。
sub-product: 站点
feature: SPA Editor，核心组件
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1976'
ht-degree: 2%

---


# 扩展核心组件{#extend-component}

了解如何扩展要与AEM SPA Editor一起使用的现有核心组件。 了解如何扩展现有组件是自定义和扩展AEM SPA Editor实现功能的一项强大技术。

## 目标

1. 扩展现有核心组件，并添加其他属性和内容。
2. 了解使用`sling:resourceSuperType`进行组件继承的基本内容。
3. 了解如何利用Sling Models的[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)来重新使用现有逻辑和功能。

## 您将构建的

在本章中，将创建新`Card`组件。 `Card`组件将扩展[Image Core Component](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html)，添加其他内容字段（如标题和行动动员按钮），以对SPA中的其他内容执行Teaser角色。

![卡组件的最终创作](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 在实际实施中，更合适的做法是简单地使用[Teaser组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/teaser.html)，然后扩展[图像核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)，以根据项目要求制作`Card`组件。 始终建议尽可能直接使用[核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)。

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

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

   如果使用[AEM 6.x](overview.md#compatibility)添加`classic`用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)安装完成的包。 将在WKND SPA上重新使用由[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的图像。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安装该包。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution)上视图完成的代码，或通过切换到分支`React/extend-component-solution`在本地签出代码。

## Inspect初始卡实施

章节启动代码提供了初始卡组件。 Inspect是卡实施的起点。

1. 在您选择的IDE中，打开`ui.apps`模块。
2. 导航到`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card`并视图`.content.xml`文件。

   ![卡组件AEM定义开始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   属性`sling:resourceSuperType`指向`wknd-spa-react/components/image`，指示`Card`组件将继承WKND SPA图像组件的所有功能。

3. Inspect文件`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   请注意，`sling:resourceSuperType`指向`core/wcm/components/image/v2/image`。 这表示WKND SPA图像组件继承了核心组件图像的所有功能。

   也称为[代理模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling资源继承是一种功能强大的设计模式，允许子组件在需要时继承功能和扩展/覆盖行为。 Sling继承支持多级继承，因此新`Card`组件最终会继承核心组件图像的功能。

   许多开发团队努力成为DRY（不要重复自己）。 Sling继承使AEM能够做到这一点。

4. 在`card`文件夹下，打开文件`_cq_dialog/.content.xml`。

   此文件是`Card`组件的组件对话框定义。 如果使用Sling继承，则可能使用[Sling资源合并](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html)的功能覆盖或扩展对话框的部分。 在此示例中，已向对话框添加了一个新选项卡，用于从作者处捕获其他数据以填充卡组件。

   `sling:orderBefore`等属性允许开发人员选择插入新选项卡或表单域的位置。 在这种情况下，`Text`选项卡将插入到`asset`选项卡之前。 要充分利用Sling资源合并，请务必了解[图像组件对话框](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)的原始对话框节点结构。

5. 在`card`文件夹下，打开文件`_cq_editConfig.xml`。 此文件指示AEM创作UI中的拖放行为。 扩展图像组件时，资源类型与组件本身匹配很重要。 查看`<parameters>`节点：

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大多数组件不需要`cq:editConfig`，图像组件的图像和子代是例外。

6. 在到`ui.frontend`模块的IDE交换机中，导航到`ui.frontend/src/components/Card`:

   ![React Component开始](assets/extend-component/react-card-component-start.png)

7. Inspect文件`Card.js`。

   已将该组件提取为使用标准`MapTo`函数映射到AEM `Card`组件。

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect方法`get imageContent()`:

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   在此示例中，我们选择通过简单地从`Card`组件传递`this.props`来重新使用现有的React Image组件`Image`。 在教程的稍后部分，将实现`get bodyContent()`方法以显示标题、日期和行动动员按钮。

## 更新模板策略

在初始`Card`实施中，查看AEM SPA Editor中的功能。 要查看初始`Card`组件，需要更新到“模板”策略。

1. 将启动代码部署到AEM的本地实例(如果您尚未：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 导航到位于[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)的SPA页面模板。
3. 更新布局容器的策略，将新`Card`组件作为允许的组件添加：

   ![更新布局容器策略](assets/extend-component/card-component-allowed.png)

   保存对策略所做的更改，并将`Card`组件作为允许的组件进行观察：

   ![卡组件作为允许的组件](assets/extend-component/card-component-allowed-layout-container.png)

## 创作初始卡组件

接下来，使用AEM SPA Editor创作`Card`组件。

1. 导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
2. 在`Edit`模式下，将`Card`组件添加到`Layout Container`:

   ![插入新组件](assets/extend-component/insert-card-component.png)

3. 将图像从资产查找器拖放到`Card`组件上：

   ![添加图像](assets/extend-component/card-add-image.png)

4. 打开`Card`组件对话框，注意到添加了&#x200B;**文本**&#x200B;选项卡。
5. 在&#x200B;**文本**&#x200B;选项卡上输入以下值：

   ![文本组件选项卡](assets/extend-component/card-component-text.png)

   **卡片路径**  — 在SPA主页下方选择一个页面。

   **CTA文本**  — “阅读更多”

   **卡片标题**  — 留空

   **从链接页面获取标题**  — 选中此复选框以指示为true。

6. 更新&#x200B;**资产元数据**&#x200B;选项卡，以添加&#x200B;**替代文本**&#x200B;和&#x200B;**题注**&#x200B;的值。

   当前，更新对话框后不显示其他更改。 要向React Component显示新字段，我们需要更新`Card`组件的Sling Model。

7. 打开新选项卡并导航到[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card)。 Inspect `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid`下的内容节点以查找`Card`组件内容。

   ![CRXDE-Lite组件属性](assets/extend-component/crxde-lite-properties.png)

   请注意，对话框会保留属性`cardPath`、`ctaText`和`titleFromPage`。

## 更新卡吊具型号

要最终将组件对话框中的值暴露给React组件，我们需要更新Sling Model，该Sling Model填充`Card`组件的JSON。 我们还有机会实施两个业务逻辑：

* 如果`titleFromPage`为&#x200B;**true**，则返回由`cardPath`指定的页面标题，否则返回`cardTitle`文本字段的值。
* 返回由`cardPath`指定的页面的上次修改日期。

返回到您选择的IDE并打开`core`模块。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`打开文件`Card.java`。

   请注意，`Card`接口当前扩展`com.adobe.cq.wcm.core.components.models.Image`，因此继承了`Image`接口的所有方法。 `Image`接口已扩展`ComponentExporter`接口，允许Sling Model导出为JSON并由SPA编辑器映射。 因此，我们不需要像在[自定义组件章节](custom-component.md)中那样显式扩展`ComponentExporter`接口。

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

   这些方法将通过JSON模型API公开并传递到React组件。

3. 打开 `CardImpl.java`. 这是`Card.java`接口的实现。 为加快教程，已经部分地研究了此实现。  请注意，使用`@Model`和`@Exporter`注释可确保Sling Model能够通过Sling Model Exporter序列化为JSON。

   `CardImpl.java` 还使用Sling [模型的委](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 派模式来避免从图像核心组件重写所有逻辑。

4. 请遵循以下规则：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述注释将根据`Card`组件的`sling:resourceSuperType`继承实例化名为`image`的Image对象。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然后，可以简单地使用`image`对象来实现由`Image`接口定义的方法，而无需自己编写逻辑。 此技术用于`getSrc()`、`getAlt()`和`getTitle()`。

5. 接下来，实施`initModel()`方法以根据`cardPath`的值启动专用变量`cardPage`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   初始化Sling模型时，始终会调用`@PostConstruct initModel()`，因此，这是初始化模型中其他方法可能使用的对象的良机。 `pageManager`是通过`@ScriptVariable`注释为Sling Models提供的许多[Java支持的全局对象](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects)中的一个。 [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-)方法在路径中采用，并返回AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html)对象；如果路径未指向有效页面，则返回null。

   这将初始化`cardPage`变量，其他新方法将使用该变量返回有关基础链接页面的数据。

6. 查看已映射到已保存创作对话框的JCR属性的全局变量。 `@ValueMapValue`注释用于自动执行映射。

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

   这些变量将用于实现`Card.java`接口的其他方法。

7. 实现在`Card.java`接口中定义的其他方法：

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
   > 您可以在此处视图[已完成的CardImpl.java](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java)。

8. 打开终端窗口，使用`core`目录中的Maven `autoInstallBundle`用户档案仅部署`core`模块的更新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   如果使用[AEM 6.x](overview.md#compatibility)添加`classic`用户档案。

9. 视图JSON模型响应：[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)并搜索`wknd-spa-react/components/card`:

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

   请注意，在`CardImpl` Sling Model中更新方法后，JSON模型会使用其他键/值对进行更新。

## 更新React组件

现在，JSON模型中填充了`ctaLinkURL`、`ctaText`、`cardTitle`和`cardLastModified`的新属性，我们可以更新React组件以显示这些属性。

1. 返回IDE并打开`ui.frontend`模块。 或者，从新的终端窗口开始webpack dev服务器，以实时查看更改：

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 在`ui.frontend/src/components/Card/Card.js`打开`Card.js`。
3. 添加方法`get ctaButton()`以呈现对操作的调用：

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

4. 为`get lastModifiedDisplayDate()`添加一种方法，将`this.props.cardLastModified`转换为表示日期的本地化字符串。

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

5. 更新`get bodyContent()`以显示`this.props.cardTitle`，并使用在前面的步骤中创建的方法：

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

6. 已在`Card.scss`添加了Sass规则，以设置标题的样式、行动动员和上次修改日期。 通过将以下行添加到文件顶部的`Card.js`，可以包括这些样式：

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > 您可以视图完成的[此处的React卡组件代码](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js)。

7. 使用Maven从项目的根部署对AEM的完整更改：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. 导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)以查看更新的组件：

   ![更新了AEM中的卡组件](assets/extend-component/updated-card-in-aem.png)

9. 您应该能够重新创作现有内容，以创建类似于以下内容的页面：

   ![卡组件的最终创作](assets/extend-component/final-authoring-card.png)

## 恭喜！{#congratulations}

恭喜您，您学习了如何使用和Sling模型及对话框来扩展AEM组件，以及如何使用JSON模型。

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution)上视图完成的代码，或通过切换到分支`React/extend-component-solution`在本地签出代码。
