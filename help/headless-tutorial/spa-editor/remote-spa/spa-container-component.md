---
title: 将可编辑的容器组件添加到远程SPA
description: 了解如何将可编辑的容器组件添加到远程SPA，以便AEM作者将组件拖放到其中。
topic: 无头、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 1%

---


# 可编辑的容器组件

[固定](./spa-fixed-component.md) 组件为创作SPA内容提供了一定的灵活性，但这种方法很僵化，需要开发人员定义可编辑内容的确切组成。为了支持作者创建出众的体验，SPA编辑器支持在SPA中使用容器组件。 容器组件允许作者将允许的组件拖放到容器中，并像在传统AEM Sites创作中一样进行创作！

![可编辑的容器组件](./assets/spa-container-component/intro.png)

在本章中，我们将在主页视图中添加一个可编辑的容器，以允许作者直接在SPA中使用AEM React核心组件来撰写和布局丰富的内容体验。

## 更新WKND应用程序

要向“主页”视图添加容器组件，请执行以下操作：

+ 导入AEM React可编辑组件的ResponsiveGrid组件
+ 导入和注册AEM React核心组件（文本和图像），以在容器组件中使用

### 在ResponsiveGrid容器组件中导入

要将可编辑区域放置到“主页”视图，我们必须：

1. 从`@adobe/aem-react-editable-components`导入ResponsiveGrid组件
1. 使用`withMappable`进行注册，以便开发人员可以将其放入SPA中
1. 此外，请使用`MapTo`进行注册，以便可以在其他容器组件中重复使用，从而有效地嵌套容器。

要执行此操作：

1. 在IDE中打开SPA项目
1. 在`src/components/aem/AEMResponsiveGrid.js`处创建React组件
1. 将以下代码添加到`AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

与[导入AEM Reach Core Components的标题组件](./spa-fixed-component.md)的代码类似。`AEMTitle.js`


`AEMResponsiveGrid.js`文件应该如下所示：

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### 使用AEMResponsiveGrid SPA组件

现在，AEM ResponsiveGrid组件已在中注册并可在SPA中使用，我们可以将其置于主页视图中。

1. 打开并编辑`react-app/src/App.js`
1. 导入`AEMResponsiveGrid`组件，并将其放在`<AEMTitle ...>`组件上方。
1. 在`<AEMResponsiveGrid...>`组件中设置以下属性
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   它会指示此`AEMResponsiveGrid`组件从AEM资源中检索其内容：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`映射到在`Remote SPA Page` AEM模板中定义的`responsivegrid`节点，并在从`Remote SPA Page` AEM模板创建的新AEM页面上自动创建。

   更新`App.js`以添加`<AEMResponsiveGrid...>`组件。

   ```
   ...
   import AEMResponsiveGrid from './components/aem/AEMResponsiveGrid';
   ...
   
   function Home() {
   return (
       <div className="Home">
           <AEMResponsiveGrid
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='root/responsivegrid'/>
   
           <AEMTitle
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='title'/>
           <Adventures />
       </div>
   );
   }
   ```

`Apps.js`文件应该如下所示：

![App.js](./assets/spa-container-component/app-js.png)

## 创建可编辑的组件

要充分发挥SPA Editor中提供的灵活创作体验容器的作用。 我们已经创建了一个可编辑的标题组件，但是让我们再做一些工作，以允许作者在新添加的容器组件中使用文本和图像AEM WCM核心组件。

### 文本组件

1. 在IDE中打开SPA项目
1. 在`src/components/aem/AEMText.js`处创建React组件
1. 将以下代码添加到`AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

`AEMText.js`文件应该如下所示：

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### 图像组件

1. 在IDE中打开SPA项目
1. 在`src/components/aem/AEMImage.js`处创建React组件
1. 将以下代码添加到`AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. 创建一个SCSS文件`src/components/aem/AEMImage.scss`，该文件为`AEMImage.scss`提供自定义样式。 这些样式以AEM React核心组件的BEM-notation CSS类为目标。
1. 将以下SCSS添加到`AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 在`AEMImage.js`中导入`AEMImage.scss`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

`AEMImage.js`和`AEMImage.scss`应该如下所示：

![AEMImage.js和AEMImage.scs](./assets/spa-container-component/aem-image-js-scss.png)

### 导入可编辑的组件

新创建的`AEMText`和`AEMImage` SPA组件在SPA中引用，并根据AEM返回的JSON动态实例化。 要确保这些组件可供SPA使用，请在`App.js`中为它们创建import语句

1. 在IDE中打开SPA项目
1. 打开文件`src/App.js`
1. 添加`AEMText`和`AEMImage`的import语句

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


结果应该如下所示：

![Apps.js](./assets/spa-container-component/app-js-imports.png)

如果这些导入是&#x200B;_未_&#x200B;添加的，则SPA将不会调用`AEMText`和`AEMImage`代码，因此组件不会针对提供的资源类型进行注册。

## 在AEM中配置容器

AEM容器组件使用策略来指示其允许的组件。 使用SPA编辑器时，这是一个关键配置，因为SPA只能呈现具有映射SPA组件对应项的AEM WCM核心组件。 确保仅允许我们为SPA实施提供的组件：

+ `AEMTitle` 映射到  `wknd-app/components/title`
+ `AEMText` 映射到  `wknd-app/components/text`
+ `AEMImage` 映射到  `wknd-app/components/image`

要配置远程SPA页面模板的reponsivegrid容器，请执行以下操作：

1. 登录到AEM作者
1. 导航到&#x200B;__工具>常规>模板> WKND应用程序__
1. 编辑&#x200B;__报表SPA页面__

   ![响应式网格策略](./assets/spa-container-component/templates-remote-spa-page.png)

1. 在右上方的模式切换器中选择&#x200B;__结构__
1. 点按以选择&#x200B;__布局容器__
1. 点按弹出栏中的&#x200B;__Policy__&#x200B;图标

   ![响应式网格策略](./assets/spa-container-component/templates-policies-action.png)

1. 在右侧的&#x200B;__允许的组件__&#x200B;选项卡下，展开&#x200B;__WKND APP - CONTENT__
1. 确保仅选择以下内容：
   + 图像
   + 文本
   + 标题

   ![远程SPA页](./assets/spa-container-component/templates-allowed-components.png)

1. 点按&#x200B;__Done__

## 在AEM中创作容器

更新了SPA以嵌入`<AEMResponsiveGrid...>`、三个AEM React核心组件（`AEMTitle`、`AEMText`和`AEMImage`）的包装器，并且更新了AEM以使用匹配的模板策略，因此我们可以开始在容器组件中创作内容。

1. 登录到AEM作者
1. 导航到&#x200B;__Sites > WKND App__
1. 点按&#x200B;__Home__，然后从顶部操作栏中选择&#x200B;__编辑__
   + 此时会显示“Hello World”文本组件，因为该组件是在从AEM项目原型生成项目时自动添加的
1. 从页面编辑器右上角的模式选择器中选择&#x200B;__编辑__
1. 在标题下方找到&#x200B;__布局容器__&#x200B;可编辑区域
1. 打开&#x200B;__页面编辑器的侧栏__，然后选择&#x200B;__组件视图__
1. 将以下组件拖到&#x200B;__布局容器__&#x200B;中
   + 图像
   + 标题
1. 拖动组件以按照以下顺序对组件重新排序：
   1. 标题
   1. 图像
   1. 文本
1. ____ 创作标题 ____ 组件
   1. 点按标题组件，然后点按&#x200B;__扳手__&#x200B;图标以&#x200B;__编辑__&#x200B;标题组件
   1. 添加以下文本：
      + 标题：__夏天就要到了，我们充分利用它！__
      + 类型：__H1__
   1. 点按&#x200B;__Done__
1. ____ 创作Imagecomponent ____ 
   1. 从图像组件上的侧栏（切换到资产视图后）将图像拖入
   1. 点按图像组件，然后点按&#x200B;__扳手__&#x200B;图标以进行编辑
   1. 选中&#x200B;__图像具有装饰性__&#x200B;复选框
   1. 点按&#x200B;__Done__
1. ____ 创作文 ____ 本组件
   1. 通过点按文本组件，然后点按&#x200B;__扳手__&#x200B;图标来编辑文本组件
   1. 添加以下文本：
      + _现在，您可以在所有为期1周的冒险中获得15%的回报，在所有为期2周或更长的冒险中获得20%的回报！在结帐时，只需添加促销活动代码SUMMERISCOMING即可获得折扣！_
   1. 点按&#x200B;__Done__

1. 现在已创作组件，但会垂直堆叠。

   ![创作的组件](./assets/spa-container-component/authored-components.png)

   使用AEM布局模式可允许我们调整组件的大小和布局。

1. 使用右上方的模式选择器切换到&#x200B;__布局模式__
1. ____ 调整图像和文本组件的大小，使它们并排显示
   + ____ Imagecomponent应为8 __列宽__
   + ____ 文本组件应 __为3列宽__

   ![布局组件](./assets/spa-container-component/layout-components.png)

1. ____ 在AEM页面编辑器中预览所做的更改
1. 刷新在[http://localhost:3000](http://localhost:3000)上本地运行的WKND应用程序，以查看创作的更改！

   ![SPA中的容器组件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已添加容器组件，该组件允许作者将可编辑的组件添加到WKND应用程序！ 您现在知道如何：

+ 在SPA中使用AEM React可编辑组件的ResponsiveGrid组件
+ 注册AEM React核心组件（文本和图像），以便通过容器组件在SPA中使用
+ 配置远程SPA页面模板，以允许启用SPA的核心组件
+ 向容器组件中添加可编辑的组件
+ SPA编辑器中的创作和布局组件

## 后续步骤

下一步将使用相同的技术将可编辑的组件添加到SPA中的Adventure Details（冒险详细信息）路由](./spa-dynamic-routes.md)。[
