---
title: 将可编辑的React容器组件添加到远程SPA
description: 了解如何将可编辑的容器组件添加到远程SPA，以便AEM作者将组件拖放到其中。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 1%

---

# 可编辑的容器组件

[固定组件](./spa-fixed-component.md)为创作SPA内容提供了一些灵活性，但这种方法很僵化，需要开发人员定义可编辑内容的确切构成。 为了支持作者创建例外体验，SPA编辑器支持在SPA中使用容器组件。 容器组件允许作者将允许的组件拖放到容器中并进行创作，就像在传统AEM Sites创作中一样！

![可编辑的容器组件](./assets/spa-container-component/intro.png)

在本章中，我们向“主页”视图添加了一个可编辑的容器，该容器允许作者直接在SPA中使用可编辑的React组件来创作和布局丰富的内容体验。

## 更新WKND应用程序

要将容器组件添加到“主页”视图，请执行以下操作：

+ 导入AEM React可编辑组件的`ResponsiveGrid`组件
+ 导入并注册自定义可编辑的React组件（文本和图像），以便在ResponsiveGrid组件中使用

### 使用ResponsiveGrid组件

要将可编辑区域添加到“主页”视图，请执行以下操作：

1. 打开并编辑`react-app/src/components/Home.js`
1. 从`@adobe/aem-react-editable-components`导入`ResponsiveGrid`组件并将其添加到`Home`组件。
1. 在`<ResponsiveGrid...>`组件上设置以下属性
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   这会指示`ResponsiveGrid`组件从AEM资源检索其内容：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`映射到在`Remote SPA Page` AEM模板中定义的`responsivegrid`节点，并在从`Remote SPA Page` AEM模板创建的新AEM页面上自动创建。

   更新`Home.js`以添加`<ResponsiveGrid...>`组件。

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

`Home.js`文件应如下所示：

![Home.js](./assets/spa-container-component/home-js.png)

## 创建可编辑的组件

充分利用SPA编辑器中提供的灵活创作体验容器。 我们已经创建了一个可编辑的标题组件，但让我们再执行一些操作，以允许作者在新添加的ResponsiveGrid组件中使用可编辑的文本和图像组件。

新的可编辑文本和图像React组件是使用在[可编辑的固定组件](./spa-fixed-component.md)中公开的可编辑组件定义模式创建的。

### 可编辑文本组件

1. 在IDE中打开SPA项目
1. 在`src/components/editable/core/Text.js`处创建React组件
1. 将以下代码添加到`Text.js`

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

1. 在`src/components/editable/EditableText.js`处创建可编辑的React组件
1. 将以下代码添加到`EditableText.js`

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

可编辑的文本组件实施应如下所示：

![可编辑文本组件](./assets/spa-container-component/text-js.png)

### 图像组件

1. 在IDE中打开SPA项目
1. 在`src/components/editable/core/Image.js`处创建React组件
1. 将以下代码添加到`Image.js`

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

1. 在`src/components/editable/EditableImage.js`处创建可编辑的React组件
1. 将以下代码添加到`EditableImage.js`

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


1. 创建一个SCSS文件`src/components/editable/EditableImage.scss`，该文件为`EditableImage.scss`提供自定义样式。 这些样式面向可编辑的React组件的CSS类。
1. 将以下SCSS添加到`EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 在`EditableImage.js`中导入`EditableImage.scss`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

可编辑的图像组件实施应如下所示：

![可编辑的图像组件](./assets/spa-container-component/image-js.png)


### 导入可编辑的组件

新创建的`EditableText`和`EditableImage` React组件在SPA中引用，并根据AEM返回的JSON动态实例化。 要确保SPA可以使用这些组件，请在`Home.js`中为其创建import语句

1. 在IDE中打开SPA项目
1. 打开文件`src/Home.js`
1. 添加`AEMText`和`AEMImage`的import语句

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

结果应如下所示：

![Home.js](./assets/spa-container-component/home-js-imports.png)

如果这些导入是&#x200B;_未添加_，则SPA不会调用`EditableText`和`EditableImage`代码，因此这些组件不会映射到提供的资源类型。

## 在AEM中配置容器

AEM容器组件使用策略来指定其允许的组件。 使用SPA编辑器时，这是一个关键配置，因为SPA只能呈现已映射AEM组件的SPA组件。 确保仅允许我们为提供的SPA实现的组件：

+ `EditableTitle`映射到`wknd-app/components/title`
+ `EditableText`映射到`wknd-app/components/text`
+ `EditableImage`映射到`wknd-app/components/image`

要配置远程SPA页模板的responsivegrid容器，请执行以下操作：

1. 登录AEM Author
1. 导航到&#x200B;__工具>常规>模板> WKND应用程序__
1. 编辑&#x200B;__报表SPA页面__

   ![响应式网格策略](./assets/spa-container-component/templates-remote-spa-page.png)

1. 在右上角的模式切换器中选择&#x200B;__结构__
1. 点击以选择&#x200B;__布局容器__
1. 点按弹出栏中的&#x200B;__策略__&#x200B;图标

   ![响应式网格策略](./assets/spa-container-component/templates-policies-action.png)

1. 在右侧的&#x200B;__允许的组件__&#x200B;选项卡下，展开&#x200B;__WKND应用程序 — 内容__
1. 确保仅选择以下选项：
   + 图像
   + 文本
   + 标题

   ![远程SPA页](./assets/spa-container-component/templates-allowed-components.png)

1. 点按&#x200B;__完成__

## 在AEM中创作容器

更新SPA以嵌入`<ResponsiveGrid...>`、三个可编辑React组件（`EditableTitle`、`EditableText`和`EditableImage`）的包装器，以及使用匹配的模板策略更新AEM之后，我们可以开始在容器组件中创作内容。

1. 登录AEM Author
1. 导航到&#x200B;__站点> WKND应用程序__
1. 点按&#x200B;__主页__&#x200B;并从顶部操作栏中选择&#x200B;__编辑__
   + 此时将显示“Hello World”文本组件，因为从AEM项目原型生成项目时，会自动添加此组件
1. 从页面编辑器右上角的模式选择器中选择&#x200B;__编辑__
1. 在标题下方找到&#x200B;__布局容器__&#x200B;可编辑区域
1. 打开&#x200B;__页面编辑器的侧栏__，然后选择&#x200B;__组件视图__
1. 将以下组件拖到&#x200B;__布局容器__&#x200B;中
   + 图像
   + 标题
1. 将组件拖动以按照以下顺序重新排序：
   1. 标题
   1. 图像
   1. 文本
1. __作者__ __标题__&#x200B;组件
   1. 点按标题组件，然后点按&#x200B;__扳手__&#x200B;图标以&#x200B;__编辑__&#x200B;标题组件
   1. 添加以下文本：
      + 标题： __夏季即将到来，让我们充分利用它！__
      + 类型： __H1__
   1. 点按&#x200B;__完成__
1. __作者__ __图像__&#x200B;组件
   1. 在图像组件上，将图像从侧栏拖入(在切换到Assets视图后)
   1. 点按图像组件，然后点按&#x200B;__扳手__&#x200B;图标以进行编辑
   1. 选中&#x200B;__图像是装饰性的__&#x200B;复选框
   1. 点按&#x200B;__完成__
1. __作者__ __文本__&#x200B;组件
   1. 通过点按文本组件并点按&#x200B;__扳手__&#x200B;图标来编辑文本组件
   1. 添加以下文本：
      + _现在，您可以享受15%的为期一周的冒险活动和20%的为期两周或更长时间的冒险活动折扣！ 在结帐时，添加促销活动代码SUMMERISCOMING以获取折扣！_
   1. 点按&#x200B;__完成__

1. 组件现已创作，但垂直栈叠。

   ![已编写的组件](./assets/spa-container-component/authored-components.png)

使用AEM布局模式可允许我们调整组件的大小和布局。

1. 使用右上角的模式选择器切换到&#x200B;__布局模式__
1. __调整图像和文本组件的大小__，使它们并排显示
   + __图像__&#x200B;组件应为&#x200B;__8列宽__
   + __文本__&#x200B;组件应为&#x200B;__3列宽__

   ![布局组件](./assets/spa-container-component/layout-components.png)

1. 在AEM页面编辑器中&#x200B;__预览__&#x200B;您所做的更改
1. 刷新在[http://localhost:3000](http://localhost:3000)上本地运行的WKND应用程序以查看所编写的更改！

   SPA中的![容器组件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已添加一个容器组件，该组件允许作者向WKND应用程序添加可编辑的组件！ 您现在知道如何：

+ 在SPA中使用AEM React可编辑组件的`ResponsiveGrid`组件
+ 创建并注册可编辑的React组件（文本和图像），以便通过容器组件在SPA中使用
+ 配置远程SPA页模板以允许启用SPA的组件
+ 将可编辑组件添加到容器组件
+ SPA编辑器中的创作和布局组件

## 后续步骤

下一步将使用此相同技术在SPA中[将可编辑组件添加到冒险详细信息路由](./spa-dynamic-routes.md)。
