---
title: 将可编辑的固定组件添加到远程SPA
description: 了解如何将可编辑的固定组件添加到远程SPA。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# 可编辑的固定组件

可编辑的React组件可以“固定”，也可以硬编码到SPA视图中。 这允许开发人员将与SPA编辑器兼容的组件放置到SPA视图中，并允许用户在AEM SPA编辑器中创作组件的内容。

![固定组件](./assets/spa-fixed-component/intro.png)

在本章中，我们将替换“主页”视图的标题“当前冒险”，它是中的硬编码文本 `Home.js` 具有固定但可编辑的标题组件。 固定组件保证标题的放置，但也允许创作标题的文本，并在开发周期之外进行更改。

## 更新WKND应用程序

添加 __固定__ “主页”视图的组件：

+ 创建自定义可编辑的标题组件并将它注册到项目标题的资源类型中
+ 将可编辑的标题组件放在“SPA主页”视图中

### 创建可编辑的React标题组件

在SPA Home视图中，替换硬编码文本 `<h2>Current Adventures</h2>` 带有自定义可编辑的标题组件。 在使用标题组件之前，我们必须：

1. 创建自定义标题React组件
1. 使用来自的方法修饰自定义标题组件 `@adobe/aem-react-editable-components` 使其可编辑。
1. 将可编辑的标题组件注册到 `MapTo` 所以可以用在 [稍后显示容器组件](./spa-container-component.md).

要执行此操作：

1. 打开远程SPA项目，位于 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` 在IDE中
1. 在上创建React组件 `react-app/src/components/editable/core/Title.js`
1. 将以下代码添加到 `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   请注意，此React组件尚无法使用AEM SPA编辑器进行编辑。 此基本组件将在下一步中变为可编辑。

   阅读有关实施详细信息的代码注释。

1. 在上创建React组件 `react-app/src/components/editable/EditableTitle.js`
1. 将以下代码添加到 `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   此 `EditableTitle` React组件包装了 `Title` React组件，封装并修饰该组件，使其可在AEM SPA编辑器中编辑。

### 使用React EditableTitle组件

现在，EditableTitle React组件已在中注册并可以在React应用程序中使用，请替换“主页”视图上的硬编码标题文本。

1. 编辑 `react-app/src/components/Home.js`
1. 在 `Home()` 在底部，导入 `EditableTitle` 并将硬编码的标题替换为新的 `AEMTitle` 组件：

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

此 `Home.js` 文件应如下所示：

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## 在AEM中创作标题组件

1. 登录AEM Author
1. 导航到 __Sites > WKND应用程序__
1. 点按 __主页__ 并选择 __编辑__ 从顶部操作栏
1. 选择 __编辑__ 从页面编辑器右上角的编辑模式选择器中
1. 将鼠标悬停在WKND徽标下方和冒险列表上方的默认标题文本上，直到显示蓝色编辑大纲
1. 点按以显示组件的操作栏，然后点按 __扳手__  编辑

   ![标题组件操作栏](./assets/spa-fixed-component/title-action-bar.png)

1. 创作标题组件：
   + 标题： __WKND冒险__
   + 类型/大小： __H2__

     ![标题组件对话框](./assets/spa-fixed-component/title-dialog.png)

1. 点按 __完成__ 以保存
1. 在AEM SPA编辑器中预览更改
1. 刷新本地运行的WKND应用程序 [http://localhost:3000](http://localhost:3000) 并会立即反映创作的标题更改。

   ![SPA中的标题组件](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已将一个固定的、可编辑的组件添加到WKND应用程序！ 您现在知道如何：

+ 在SPA中创建了一个固定的可编辑组件
+ 在AEM中创作固定组件
+ 在远程SPA中查看创作的内容

## 后续步骤

接下来的步骤是 [添加AEM ResponsiveGrid容器组件](./spa-container-component.md) 添加到SPA，以便作者向SPA添加和可编辑的组件！
