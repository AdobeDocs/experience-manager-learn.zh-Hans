---
title: 创建自定义组件 | AEM SPA编辑器和React入门
description: 了解如何创建要与AEM SPA编辑器一起使用的自定义组件。 了解如何开发创作对话框和Sling Models以扩展JSON模型以填充自定义组件。
sub-product: 站点
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
translation-type: tm+mt
source-git-commit: e6da018a21155eca3a52dd562e469296b3c68c0d
workflow-type: tm+mt
source-wordcount: '1486'
ht-degree: 1%

---


# 创建自定义组件{#custom-component}

了解如何创建要与AEM SPA编辑器一起使用的自定义组件。 了解如何开发创作对话框和Sling Models以扩展JSON模型以填充自定义组件。

## 目标

1. 了解Sling Models在操作AEM提供的JSON模型API中的角色。
2. 了解如何创建新的AEM组件对话框。
3. 了解如何创建与SPA编辑器框架兼容的&#x200B;**custom** AEM组件。

## 您将构建的内容

前几章的重点是开发SPA组件并将它们映射到&#x200B;*现有的* AEM核心组件。 本章将重点介绍如何创建和扩展&#x200B;*new* AEM组件以及如何操作AEM提供的JSON模型。

简单的`Custom Component`说明了创建net-new AEM组件所需的步骤。

![全部大写中显示的消息](assets/custom-component/message-displayed.png)

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility) ，则添加`classic`用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 为传统[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)安装完成的软件包。 由[WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的图像将重新用于WKND SPA。 可以使用[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安装该包。

   ![包管理器安装wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution)上视图完成的代码，或通过切换到分支`React/custom-component-solution`在本地签出代码。

## 定义AEM组件

AEM组件定义为节点和属性。 在项目中，这些节点和属性在`ui.apps`模块中表示为XML文件。 然后，在`ui.apps`模块中创建AEM组件。

>[!NOTE]
>
> 有关AEM组件[基础知识的快速复习可能会很有帮助。](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)

1. 在您选择的IDE中，打开`ui.apps`文件夹。
2. 导航到`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components`并新建一个名为`custom-component`的文件夹。
3. 在`custom-component`文件夹下新建一个名为`.content.xml`的文件。 使用以下内容填充`custom-component/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![创建自定义组件定义](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` -标识此节点将是AEM组件。

   `jcr:title` 是将显示给内容作者的值，它确 `componentGroup` 定在创作UI中对组件进行分组。

4. 在`custom-component`文件夹下，创建另一个名为`_cq_dialog`的文件夹。
5. 在`_cq_dialog`文件夹下，新建一个名为`.content.xml`的文件，并用以下内容填充该文件：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![自定义组件定义](assets/custom-component/dialog-custom-component-defintion.png)

   上述XML文件为`Custom Component`生成一个非常简单的对话框。 文件的关键部分是内部`<message>`节点。 此对话框将包含一个名为`Message`的简单`textfield`，并将textifeld的值保留到名为`message`的属性中。

   接下来将创建一个Sling Model，通过JSON模型显示`message`属性的值。

   >[!NOTE]
   >
   > 通过查看核心组件定义](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)，您可以视图更多[对话框示例。 您还可以视图其他表单字段，如`select`、`textarea`、`pathfield`，可在[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)中的`/libs/granite/ui/components/coral/foundation/form`下找到。

   对于传统的AEM组件，通常需要[HTL](https://docs.adobe.com/content/help/zh-Hans/experience-manager-htl/using/overview.html)脚本。 由于SPA将呈现组件，因此不需要HTL脚本。

## 创建Sling模型

Sling Models是注释驱动的Java“POJO”(Plain Old Java Objects)，可促进数据从JCR到Java变量的映射。 [Sling Model](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) 通常能够封装AEM组件复杂的服务器端业务逻辑。

在SPA Editor的上下文中，Sling Models使用[Sling Model Exporter](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)通过功能通过JSON模型公开组件内容。

1. 在您选择的IDE中，打开`core`模块。 `CustomComponent.java` 已 `CustomComponentImpl.java` 经作为章节启动代码的一部分创建和发布。

   >[!NOTE]
   >
   > 如果使用Visual Studio代码IDE，则安装Java的[扩展可能会很有帮助。](https://code.visualstudio.com/docs/java/extensions)

2. 打开位于`core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`的Java接口`CustomComponent.java`:

   ![CustomComponent.java接口](assets/custom-component/custom-component-interface.png)

   这是将由Sling Model实现的Java接口。

3. 更新`CustomComponent.java`，以扩展`ComponentExporter`接口：

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   实施`ComponentExporter`接口是JSON模型API自动选取Sling模型的要求。

   `CustomComponent`接口包括单个getter方法`getMessage()`。 这是通过JSON模型显示创作对话框值的方法。 在JSON模型中，将只导出参数为`()`的公共getter方法。

4. 打开`core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`处的`CustomComponentImpl.java`。

   这是`CustomComponent`接口的实现。 `@Model`注释将Java类标识为Sling Model。 `@Exporter`注释使Java类能够通过Sling Model Exporter进行序列化和导出。

5. 更新静态变量`RESOURCE_TYPE`以指向在上一个练习中创建的AEM组件`wknd-spa-react/components/custom-component`。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   组件的资源类型是将Sling Model绑定到AEM组件并最终映射到React组件的类型。

6. 将`getExportedType()`方法添加到`CustomComponentImpl`类以返回组件资源类型：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   实现`ComponentExporter`接口时需要此方法，并将公开允许映射到React组件的资源类型。

7. 更新`getMessage()`方法以返回作者对话框保留的`message`属性的值。 使用`@ValueMap`注释将JCR值`message`映射到Java变量：

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   添加一些附加的“业务逻辑”以返回包含所有大写字母的消息的字符串值。 这将允许我们查看作者对话框存储的原始值与Sling Model公开的值之间的差异。

   >[!NOTE]
   >
   > 您可以在此处视图[已完成的CustomComponentImpl.java](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java)。

## 更新React组件

已创建自定义组件的反应代码。 接下来，进行一些更新以将React组件映射到AEM组件。

1. 在`ui.frontend`模块中，打开文件`ui.frontend/src/components/Custom/Custom.js`。
2. 将`{this.props.message}`变量作为`render()`方法的一部分进行观察：

   ```js
   return (
           <div className="CustomComponent">
               <h2 className="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   应将来自Sling Model的转换后的大写值映射到此`message`属性。

3. 从AEM SPA Editor JS SDK导入`MapTo`对象，并使用它映射到AEM组件：

   ```diff
   + import {MapTo} from '@adobe/aem-react-editable-components';
   
    ...
    export default class Custom extends Component {
        ...
    }
   
   + MapTo('wknd-spa-react/components/custom-component')(Custom, CustomEditConfig);
   ```

4. 使用您的Maven技能从项目目录的根部部署到本地AEM环境的所有更新：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新模板策略

然后，导航到AEM以验证更新并允许将`Custom Component`添加到SPA。

1. 导航至[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)验证新Sling Model的注册。

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您应当看到以上两行，它们指示`CustomComponentImpl`与`wknd-spa-react/components/custom-component`组件关联，并且它已通过Sling Model Exporter注册。

2. 导航到位于[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)的SPA页面模板。
3. 更新布局容器的策略，将新`Custom Component`添加为允许的组件：

   ![更新布局容器策略](assets/custom-component/custom-component-allowed.png)

   保存对策略所做的更改，并将`Custom Component`作为允许的组件进行观察：

   ![自定义组件作为允许的组件](assets/custom-component/custom-component-allowed-layout-container.png)

## 创作自定义组件

然后，使用AEM SPA编辑器创作`Custom Component`。

1. 导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
2. 在`Edit`模式下，将`Custom Component`添加到`Layout Container`:

   ![插入新组件](assets/custom-component/insert-custom-component.png)

3. 打开组件的对话框，并输入包含一些小写字母的消息。

   ![配置自定义组件](assets/custom-component/enter-dialog-message.png)

   这是根据本章前面的XML文件创建的对话框。

4. 保存更改。请注意显示的消息是大写的。

   ![全部大写中显示的消息](assets/custom-component/message-displayed.png)

5. 视图JSON模型，方法是导航到[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 搜索 `wknd-spa-react/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   请注意，JSON值根据添加到Sling Model的逻辑设置为所有大写字母。

## 恭喜！{#congratulations}

恭喜您，您学习了如何创建要与SPA编辑器一起使用的自定义AEM组件。 您还学习了对话框、JCR属性和Sling模型如何交互以输出JSON模型。

您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution)上视图完成的代码，或通过切换到分支`React/custom-component-solution`在本地检出代码。

### 后续步骤{#next-steps}

[扩展核心组件](extend-component.md) -了解如何扩展现有核心组件以与AEM SPA编辑器一起使用。了解如何向现有组件添加属性和内容是扩展AEM SPA Editor实现功能的强大技术。
