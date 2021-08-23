---
title: 创建自定义天气组件 | AEM SPA Editor和React快速入门
description: 了解如何创建要与AEM SPA编辑器一起使用的自定义天气组件。 了解如何开发创作对话框和Sling模型以扩展JSON模型以填充自定义组件。 使用开放天气API和React开放天气组件。
sub-product: 站点
feature: SPA编辑器
doc-type: tutorial
topics: development
version: cloud-service
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 1%

---


# 创建自定义WeatherComponent {#custom-component}

了解如何创建要与AEM SPA编辑器一起使用的自定义天气组件。 了解如何开发创作对话框和Sling模型以扩展JSON模型以填充自定义组件。 使用[Open Weather API](https://openweathermap.org)和[React Open Weather组件](https://www.npmjs.com/package/react-open-weather)。

## 目标

1. 了解Sling模型在处理AEM提供的JSON模型API中的角色。
2. 了解如何创建新的AEM组件对话框。
3. 了解如何创建与SPA编辑器框架兼容的&#x200B;**custom** AEM组件。

## 将构建的内容

将构建一个简单的天气组件。 内容作者将能够将此组件添加到SPA中。 使用AEM对话框，作者可以设置天气的显示位置。  此组件的实施说明了创建与AEM SPA Editor框架兼容的全新AEM组件所需的步骤。

![配置打开的天气组件](assets/custom-component/enter-dialog.png)

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 本章是[导航和路由](navigation-routing.md)章节的继续，但是，随后您只需要将一个启用了SPA的AEM项目部署到本地AEM实例。

### 打开天气API密钥

在本教程的后面，需要[Open Weather](https://openweathermap.org/)中的API密钥。 [对于有限](https://home.openweathermap.org/users/sign_up) 量的API调用，注册是免费的。

## 定义AEM组件

AEM组件被定义为节点和属性。 在项目中，这些节点和属性在`ui.apps`模块中表示为XML文件。 接下来，在`ui.apps`模块中创建AEM组件。

>[!NOTE]
>
> 有关AEM组件[基础知识的快速刷新器可能](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html)很有用。

1. 在选择的IDE中，打开`ui.apps`文件夹。
2. 导航到`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components`并创建一个名为`open-weather`的新文件夹。
3. 在`open-weather`文件夹下创建一个名为`.content.xml`的新文件。 使用以下内容填充`open-weather/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![创建自定义组件定义](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — 标识此节点将是AEM组件。

   `jcr:title` 是将显示给内容作者的值，它可确 `componentGroup` 定创作UI中的组件分组。

4. 在`custom-component`文件夹下，创建另一个名为`_cq_dialog`的文件夹。
5. 在`_cq_dialog`文件夹下，创建一个名为`.content.xml`的新文件，并在其中填充以下内容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
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
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
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

   上述XML文件为`Weather Component`生成一个非常简单的对话框。 文件的关键部分是内部`<label>`、`<lat>`和`<lon>`节点。 此对话框将包含两个`numberfield`s和一个`textfield`，用户可以配置要显示的天气。

   将创建一个Sling模型，以便通过JSON模型公开`label`、`lat`和`long`属性的值。

   >[!NOTE]
   >
   > 您可以通过查看核心组件定义](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)来查看更多[对话框示例。 您还可以查看其他表单字段，如`select`、`textarea`、`pathfield`，它们位于[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)中`/libs/granite/ui/components/coral/foundation/form`下方。

   对于传统AEM组件，通常需要[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=zh-Hans)脚本。 由于SPA将渲染组件，因此无需HTL脚本。

## 创建Sling模型

Sling模型是注释驱动的Java“POJO”（纯旧Java对象），有助于将数据从JCR映射到Java变量。 [Sling Models](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) 通常可以函数为AEM组件封装复杂的服务器端业务逻辑。

在SPA编辑器的上下文中，Sling模型通过使用[Sling模型导出器](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)的功能，通过JSON模型公开组件的内容。

1. 在选择的IDE中，打开`core`模块（位于`aem-guides-wknd-spa.react/core`）。
1. 在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`创建名为`OpenWeatherModel.java`的文件。
1. 使用以下内容填充`OpenWeatherModel.java` :

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
   
       public String getLabel();
   
       public double getLat();
   
       public double getLon();
   
   }
   ```

   这是我们组件的Java界面。 为了使我们的Sling模型与SPA Editor框架兼容，它必须扩展`ComponentExporter`类。

1. 在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`下创建名为`impl`的文件夹。
1. 在`impl`下创建名为`OpenWeatherModelImpl.java`的文件，然后使用以下内容填充：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
       )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
       )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   静态变量`RESOURCE_TYPE`必须指向组件`ui.apps`中的路径。 `getExportedType()`用于通过`MapTo`将JSON属性映射到SPA组件。 `@ValueMapValue` 是读取对话框保存的jcr属性的注释。

## 更新SPA

接下来，更新React代码以包含[React Open Weather组件](https://www.npmjs.com/package/react-open-weather)，并将其映射到在上一步中创建的AEM组件。

1. 将React Open Weather组件安装为&#x200B;**npm**&#x200B;依赖项：

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. 在`ui.frontend/src/components/OpenWeather`创建名为`OpenWeather`的新文件夹。
1. 添加名为`OpenWeather.js`的文件，然后使用以下内容填充该文件：

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if(OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. 在`ui.frontend/src/components/import-components.js`更新`import-components.js`以包含`OpenWeather`组件：

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. 使用您的Maven技能，从项目目录的根目录将所有更新部署到本地AEM环境：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新模板策略

接下来，导航到AEM以验证更新并允许将`OpenWeather`组件添加到SPA。

1. 导航到[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)，以验证新Sling模型的注册情况。

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   您应会看到以上两行代码，指示`OpenWeatherModelImpl`与`wknd-spa-react/components/open-weather`组件关联，并且已通过Sling模型导出程序进行注册。

1. 导航到位于[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)的SPA页面模板。
1. 更新布局容器的策略，将新的`Open Weather`添加为允许的组件：

   ![更新布局容器策略](assets/custom-component/custom-component-allowed.png)

   保存对策略所做的更改，并将`Open Weather`作为允许的组件进行观察：

   ![自定义组件作为允许的组件](assets/custom-component/custom-component-allowed-layout-container.png)

## 创作打开的天气组件

接下来，使用AEM SPA编辑器创作`Open Weather`组件。

1. 导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
1. 在`Edit`模式下，将`Open Weather`添加到`Layout Container`:

   ![插入新组件](assets/custom-component/insert-custom-component.png)

1. 打开组件的对话框，然后输入&#x200B;**标签**、**纬度**&#x200B;和&#x200B;**经度**。 例如&#x200B;**San Diego**、**32.7157**&#x200B;和&#x200B;**-117.1611**。 使用开放天气API，西半球和南半球的数字表示为负数

   ![配置打开的天气组件](assets/custom-component/enter-dialog.png)

   这是基于章节前面的XML文件创建的对话框。

1. 保存更改。请注意，**圣地亚哥**&#x200B;的天气现在已显示：

   ![天气组件已更新](assets/custom-component/weather-updated.png)

1. 通过导航到[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，查看JSON模型。 搜索 `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   JSON值由Sling模型输出。 这些JSON值将作为prop传递到React组件中。

## 恭喜！ {#congratulations}

恭喜，您已学会如何创建要与SPA编辑器一起使用的自定义AEM组件。 您还了解了对话框、JCR属性和Sling模型如何进行交互以输出JSON模型。

### 后续步骤 {#next-steps}

[扩展核心组件](extend-component.md)  — 了解如何扩展要与AEM SPA编辑器一起使用的现有AEM核心组件。了解如何向现有组件添加属性和内容是一项功能强大的技术，可扩展AEM SPA Editor实施的功能。
