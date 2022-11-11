---
title: 将可编辑的React容器组件添加到远程SPA
description: 了解如何将可编辑的容器组件添加到远程SPA，以便AEM作者将组件拖放到其中。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 1%

---

# 可编辑的容器组件

[固定组件](./spa-fixed-component.md) 为创作SPA内容提供了一定的灵活性，但这种方法很严格，需要开发人员定义可编辑内容的确切组成。 为了支持作者创建出众的体验，SPA编辑器支持在SPA中使用容器组件。 容器组件允许作者将允许的组件拖放到容器中，并像在传统AEM Sites创作中一样进行创作！

![可编辑的容器组件](./assets/spa-container-component/intro.png)

在本章中，我们向主页视图中添加一个可编辑的容器，以允许作者直接在SPA中使用可编辑的React组件来撰写和布局丰富的内容体验。

## 更新WKND应用程序

要向“主页”视图添加容器组件，请执行以下操作：

+ 导入AEM React可编辑组件的 `ResponsiveGrid` 组件
+ 导入并注册自定义可编辑的React组件（文本和图像），以在ResponsiveGrid组件中使用

### 使用ResponsiveGrid组件

要向“主页”视图添加可编辑区域，请执行以下操作：

1. 打开和编辑 `react-app/src/components/Home.js`
1. 导入 `ResponsiveGrid` 组件从 `@adobe/aem-react-editable-components` 并将其添加到 `Home` 组件。
1. 在 `<ResponsiveGrid...>` 组件
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   这会指示 `ResponsiveGrid` 用于从AEM资源检索其内容的组件：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   的 `itemPath` 映射到 `responsivegrid` 在 `Remote SPA Page` AEM模板，将在通过 `Remote SPA Page` AEM模板。

   更新 `Home.js` 添加 `<ResponsiveGrid...>` 组件。

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

的 `Home.js` 文件应该如下所示：

![Home.js](./assets/spa-container-component/home-js.png)

## 创建可编辑的组件

要充分发挥SPA Editor中提供的灵活创作体验容器的作用。 我们已经创建了一个可编辑的标题组件，但是让我们再做一些工作，以允许作者在新添加的ResponsiveGrid组件中使用可编辑的文本和图像组件。

新的可编辑文本和图像反应组件是使用 [可编辑的固定组件](./spa-fixed-component.md).

### 可编辑的文本组件

1. 在IDE中打开SPA项目
1. 在 `src/components/editable/core/Text.js`
1. 将以下代码添加到 `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. 在以下位置创建可编辑的React组件 `src/components/editable/EditableText.js`
1. 将以下代码添加到 `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

可编辑的文本组件实施应当类似于：

![可编辑的文本组件](./assets/spa-container-component/text-js.png)

### 图像组件

1. 在IDE中打开SPA项目
1. 在 `src/components/editable/core/Image.js`
1. 将以下代码添加到 `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. 在以下位置创建可编辑的React组件 `src/components/editable/EditableImage.js`
1. 将以下代码添加到 `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. 创建SCSS文件 `src/components/editable/EditableImage.scss` 为 `EditableImage.scss`. 这些样式将定位可编辑React组件的CSS类。
1. 将以下SCSS添加到 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 导入 `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

可编辑的图像组件实施应类似于：

![可编辑的图像组件](./assets/spa-container-component/image-js.png)


### 导入可编辑的组件

新创建的 `EditableText` 和 `EditableImage` React组件在SPA中引用，并且会根据AEM返回的JSON进行动态实例化。 要确保这些组件可供SPA使用，请在 `Home.js`

1. 在IDE中打开SPA项目
1. 打开文件 `src/Home.js`
1. 为添加import语句 `AEMText` 和 `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

结果应该如下所示：

![Home.js](./assets/spa-container-component/home-js-imports.png)

如果这些导入是 _not_ 添加了 `EditableText` 和 `EditableImage` SPA不会调用代码，因此组件不会映射到提供的资源类型。

## 在AEM中配置容器

AEM容器组件使用策略来指示其允许的组件。 使用SPA编辑器时，这是一个关键配置，因为只有映射了SPA组件对应项的AEM组件才可由SPA呈现。 确保仅允许我们为SPA实施提供的组件：

+ `EditableTitle` 映射到 `wknd-app/components/title`
+ `EditableText` 映射到 `wknd-app/components/text`
+ `EditableImage` 映射到 `wknd-app/components/image`

要配置远程SPA页面模板的reponsivegrid容器，请执行以下操作：

1. 登录到AEM作者
1. 导航到 __工具>常规>模板> WKND应用程序__
1. 编辑 __报表SPA页面__

   ![响应式网格策略](./assets/spa-container-component/templates-remote-spa-page.png)

1. 选择 __结构__ 在右上方的模式切换器中
1. 点按以选择 __布局容器__
1. 点按 __策略__ 图标

   ![响应式网格策略](./assets/spa-container-component/templates-policies-action.png)

1. 在右侧，在 __允许的组件__ 选项卡，展开 __WKND应用程序 — 内容__
1. 确保仅选择以下内容：
   + 图像
   + 文本
   + 标题

   ![远程SPA页](./assets/spa-container-component/templates-allowed-components.png)

1. 点按 __完成__

## 在AEM中创作容器

更新SPA以嵌入 `<ResponsiveGrid...>`，三个可编辑的React组件的包装器(`EditableTitle`, `EditableText`和 `EditableImage`)，且AEM已更新为匹配的模板策略，因此我们可以开始在容器组件中创作内容。

1. 登录到AEM作者
1. 导航到 __站点> WKND应用程序__
1. 点按 __主页__ 选择 __编辑__ 从顶部操作栏
   + 此时会显示“Hello World”文本组件，因为该组件是在从AEM项目原型生成项目时自动添加的
1. 选择 __编辑__ 从页面编辑器右上角的模式选择器中
1. 找到 __布局容器__ “标题”下方的可编辑区域
1. 打开 __页面编辑器的侧栏__，然后选择 __组件视图__
1. 将以下组件拖入 __布局容器__
   + 图像
   + 标题
1. 拖动组件以按照以下顺序对组件重新排序：
   1. 标题
   1. 图像
   1. 文本
1. __作者__ the __标题__ 组件
   1. 点按标题组件，然后点按 __扳手__ 图标 __编辑__ 标题组件
   1. 添加以下文本：
      + 标题： __夏天来了，我们尽量利用它！__
      + 类型： __H1__
   1. 点按 __完成__
1. __作者__ the __图像__ 组件
   1. 从图像组件上的侧栏（切换到资产视图后）将图像拖入
   1. 点按图像组件，然后点按 __扳手__ 图标进行编辑
   1. 检查 __图像具有装饰性__ 复选框
   1. 点按 __完成__
1. __作者__ the __文本__ 组件
   1. 通过点按文本组件，然后点按 __扳手__ 图标
   1. 添加以下文本：
      + _现在，您可以在所有为期1周的冒险中获得15%的回报，在所有为期2周或更长的冒险中获得20%的回报！ 在结帐时，添加促销活动代码SUMMERISCOMING以获得折扣！_
   1. 点按 __完成__

1. 现在已创作组件，但会垂直堆叠。

   ![创作的组件](./assets/spa-container-component/authored-components.png)

使用AEM布局模式可允许我们调整组件的大小和布局。

1. 切换到 __布局模式__ 使用右上角的模式选择器
1. __调整大小__ 图像和文本组件，以便它们可并排显示
   + __图像__ 组件应 __宽8列__
   + __文本__ 组件应 __宽3列__

   ![布局组件](./assets/spa-container-component/layout-components.png)

1. __预览__ 您对AEM页面编辑器所做的更改
1. 刷新在本地运行的WKND应用程序 [http://localhost:3000](http://localhost:3000) 查看创作的更改！

   ![SPA中的容器组件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已添加容器组件，该组件允许作者将可编辑的组件添加到WKND应用程序！ 您现在知道如何：

+ 使用AEM React可编辑组件的 `ResponsiveGrid` 组件在SPA中
+ 创建并注册可编辑的React组件（文本和图像），以便通过容器组件在SPA中使用
+ 配置远程SPA页面模板以允许启用SPA的组件
+ 向容器组件中添加可编辑的组件
+ SPA编辑器中的创作和布局组件

## 后续步骤

下一步将使用相同的技术 [将可编辑的组件添加到冒险详细信息路径](./spa-dynamic-routes.md) 中。
