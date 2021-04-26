---
title: 将可编辑的固定组件添加到远程SPA
description: 了解如何将可编辑的固定组件添加到远程SPA。
topic: 无外设、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 1%

---


# 可编辑的固定组件

可编辑的React组件可以是“固定”的，也可以硬编码到SPA视图中。 这允许开发人员将SPA Editor兼容组件放入SPA视图中，并允许用户在AEM SPA Editor中创作组件的内容。

![固定组件](./assets/spa-fixed-component/intro.png)

在本章中，我们将主页视图的标题“当前历险记”替换为固定但可编辑的标题组件，该标题组件是`Home.js`中硬编码的文本。 固定组件可保证标题的位置，但也允许创作标题的文本并在开发周期之外进行更改。

## 更新WKND应用程序

要将“固定”组件添加到“主页”视图，请执行以下操作：

+ 导入AEM React核心组件标题组件，并将其注册到项目标题的资源类型
+ 将可编辑的标题组件放在SPA主页视图上

### 在AEM React Core组件的标题组件中导入

在SPA主页视图中，将硬编码文本`<h2>Current Adventures</h2>`替换为AEM React Core Components的标题组件。 在使用标题组件之前，我们必须：

1. 从`@adobe/aem-core-components-react-base`导入标题组件
1. 使用`withMappable`进行注册，以便开发人员将其放入SPA
1. 另外，请注册`MapTo`，以便在[以后](./spa-container-component.md)的容器组件中使用。

要执行此操作：

1. 在IDE中的`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`处打开远程SPA项目
1. 在`react-app/src/components/aem/AEMTitle.js`创建React组件
1. 将以下代码添加到`AEMTitle.js`。

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

阅读代码中的注释，了解实施详细信息。

`AEMTitle.js`文件应该如下：

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### 使用React AEMTitle组件

现在，AEM React Core组件的标题组件已注册并可在React应用程序中使用，请替换主视图上硬编码的标题文本。

1. 编辑 `react-app/src/App.js`
1. 在底部的`Home()`中，将硬编码标题替换为新的`AEMTitle`组件：

   ```
   <h2>Current Adventures</h2>
   ```

   替换为

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   使用以下代码更新`Apps.js`:

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Apps.js`文件应该如下：

![App.js](./assets/spa-fixed-component/app-js.png)

## 在AEM中创作标题组件

1. 登录到AEM作者
1. 导航到&#x200B;__站点> WKND应用程序__
1. 点按&#x200B;__主页__，然后从顶部操作栏中选择&#x200B;__编辑__
1. 从页面编辑器右上角的编辑模式选择器中选择&#x200B;__编辑__
1. 将鼠标悬停在WKND徽标下方和冒险列表上方的默认标题文本上，直到显示蓝色编辑轮廓
1. 点按以显示组件的操作栏，然后点按&#x200B;__扳手__&#x200B;进行编辑

   ![标题组件操作栏](./assets/spa-fixed-component/title-action-bar.png)

1. 创作标题组件：
   + 标题：__WKND冒险__
   + 类型/大小：__H2__

      ![标题组件对话框](./assets/spa-fixed-component/title-dialog.png)

1. 点按&#x200B;__完成__&#x200B;以保存
1. 预览在AEM SPA Editor中所做的更改
1. 刷新在[http://localhost:3000](http://localhost:3000)上本地运行的WKND应用程序，并立即看到所创作的标题更改。

   ![SPA中的标题组件](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已向WKND应用程序添加了一个固定的、可编辑的组件！ 您现在知道如何：

+ 在SPA中导入并重复使用AEM React Core组件
+ 向SPA添加固定但可编辑的组件
+ 在AEM中创作固定组件
+ 在远程SPA中查看创作的内容

## 下面的步骤

接下来的步骤是[将AEM ResponsiveGrid容器组件](./spa-container-component.md)添加到SPA中，使作者能够向SPA中添加和可编辑组件！
