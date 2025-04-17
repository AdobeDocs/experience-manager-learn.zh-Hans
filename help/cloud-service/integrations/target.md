---
title: 集成AEM Headless和Target
description: 了解如何集成AEM Headless和Adobe Target，以使用Experience Platform Web SDK个性化Headless体验。
version: Experience Manager as a Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: adc2f352544b4718522073642c6bf971b3600616
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# 集成AEM Headless和Target

了解如何将AEM Headless与Adobe Target集成，方法是将AEM内容片段导出到Adobe Target，并使用它们通过Adobe Experience Platform Web SDK的alloy.js个性化Headless体验。 [React WKND应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html?lang=zh-Hans)用于探索如何将使用内容片段选件的个性化Target活动添加到体验中，以促进WKND冒险。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教程介绍了设置AEM和Adobe Target所涉及的步骤：

1. 在AEM Author中[为Adobe Target创建Adobe IMS配置](#adobe-ims-configuration)
2. 在Adobe Target Author中[创建AEM Cloud Service](#adobe-target-cloud-service)
3. 在Adobe Target Author中[将AEM Cloud Service应用到AEM Assets文件夹](#configure-asset-folders)
4. 在Adobe Admin Console中[权限Adobe Target Cloud Service](#permission)
5. [将内容片段](#export-content-fragments)从AEM Author导出到Target
6. [在Adobe Target中使用内容片段选件](#activity)创建活动
7. 在Experience Platform中[创建Experience Platform数据流](#datastream-id)
8. [使用Adobe Web SDK将个性化集成到基于React的AEM Headless应用程序中](#code)。

## Adobe IMS配置{#adobe-ims-configuration}

Adobe IMS配置有助于在AEM和Adobe Target之间进行身份验证。

查看[文档](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/integrations/target#adobe-target-cloud-service)以了解有关如何创建Adobe IMS配置的分步说明。

## Adobe Target Cloud Service{#adobe-target-cloud-service}

在AEM中创建Adobe Target Cloud Service，以便于将内容片段导出到Adobe Target。

查看[文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)以了解有关如何创建Adobe Target Cloud Service的分步说明。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 配置资源文件夹{#configure-asset-folders}

在上下文感知配置中配置的Adobe Target Cloud Service必须应用于包含要导出到Adobe Target的内容片段的AEM Assets文件夹层次结构。

+++有关分步说明，请展开

1. 以DAM管理员身份登录到&#x200B;__AEM创作服务__
1. 导航到&#x200B;__Assets >文件__，找到已应用`/conf`的资源文件夹
1. 选择资产文件夹，然后从顶部操作栏中选择&#x200B;__属性__
1. 选择&#x200B;__云服务__&#x200B;选项卡
1. 确保将云配置设置为包含Adobe Target云服务配置的上下文感知配置(`/conf`)。
1. 从&#x200B;__Adobe Target配置__&#x200B;下拉列表中选择&#x200B;__Cloud Service__。
1. 选择右上方的&#x200B;__保存并关闭__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## 许可AEM Target集成{#permission}

Adobe Target集成(显示为developer.adobe.com项目)必须被授予Adobe Admin Console中的&#x200B;__编辑者__&#x200B;产品角色，才能将内容片段导出到Adobe Target。

+++有关分步说明，请展开

1. 以可在Adobe Admin Console中管理Experience Cloud产品的用户身份登录Adobe Target
1. 打开[Adobe Admin Console](https://adminconsole.adobe.com)
1. 选择&#x200B;__产品__，然后打开&#x200B;__Adobe Target__
1. 在&#x200B;__产品配置文件__&#x200B;选项卡上，选择&#x200B;__*默认工作区*__
1. 选择&#x200B;__API凭据__&#x200B;选项卡
1. 在此列表中找到您的developer.adobe.com应用，并将其&#x200B;__产品角色__&#x200B;设置为&#x200B;__编辑者__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 将内容片段导出到目标{#export-content-fragments}

存在于[配置的AEM Assets文件夹层次结构](#apply-adobe-target-cloud-service-to-aem-assets-folders)下的内容片段可以作为内容片段选件导出到Adobe Target。 这些内容片段选件（Target中特殊形式的JSON选件）可以在Target活动中使用，以在Headless应用程序中提供个性化体验。

+++有关分步说明，请展开

1. 以DAM用户身份登录到&#x200B;__AEM作者__
1. 导航到&#x200B;__Assets >文件__，并在“启用Adobe Target”文件夹下找到要导出为JSON到Target的内容片段
1. 选择要导出到Adobe Target的内容片段
1. 从顶部操作栏中选择&#x200B;__导出到Adobe Target选件__
   + 此操作将内容片段的完全水合JSON表示形式作为“内容片段选件”导出到Adobe Target
   + 可以在AEM中查看完全水合的JSON表示形式
      + 选择内容片段
      + 展开侧面板
      + 在左侧面板中选择&#x200B;__预览__&#x200B;图标
      + 导出到Adobe Target的JSON表示形式将显示在主视图中
1. 使用Adobe Target编辑器角色中的用户登录[Adobe Experience Cloud](https://experience.adobe.com)
1. 从[Experience Cloud](https://experience.adobe.com)中，从右上方的产品切换器中选择&#x200B;__Target__&#x200B;以打开Adobe Target。
1. 确保在右上方的&#x200B;__Workspace切换器__&#x200B;中选择默认Workspace。
1. 在顶部导航中选择&#x200B;__选件__&#x200B;选项卡
1. 选择&#x200B;__类型__&#x200B;下拉列表，然后选择&#x200B;__内容片段__
1. 验证从AEM导出的内容片段是否显示在列表中
   + 将鼠标悬停在选件上，然后选择&#x200B;__查看__&#x200B;按钮
   + 查看&#x200B;__选件信息__&#x200B;并查看&#x200B;__AEM深层链接__，该链接直接在AEM创作服务中打开内容片段

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用内容片段选件的Target活动{#activity}

在Adobe Target中，可创建一个使用内容片段选件JSON作为内容的活动，允许在Headless应用程序中通过AEM中创建和管理内容提供个性化体验。

在本例中，我们使用简单的A/B活动，但可以使用任何Target活动。

+++有关分步说明，请展开

1. 在顶部导航中选择&#x200B;__活动__&#x200B;选项卡
1. 选择&#x200B;__+创建活动__，然后选择要创建的活动类型。
   + 此示例创建一个简单的&#x200B;__A/B测试__，但内容片段选件可以为任何活动类型提供支持
1. 在&#x200B;__创建活动__&#x200B;向导中
   + 选择&#x200B;__Web__
   + 在&#x200B;__选择体验编辑器__&#x200B;中，选择&#x200B;__表单__
   + 在&#x200B;__选择Workspace__&#x200B;中，选择&#x200B;__默认Workspace__
   + 在&#x200B;__选择属性__&#x200B;中，选择活动可用的属性，或选择&#x200B;__无属性限制__&#x200B;以允许在所有属性中使用它。
   + 选择&#x200B;__下一步__&#x200B;以创建该活动
1. 通过选择左上角的&#x200B;__重命名__&#x200B;重命名该活动
   + 为活动提供一个有意义的名称
1. 在初始体验中，为要定位的活动设置&#x200B;__位置1__
   + 在此示例中，定位名为`wknd-adventure-promo`的自定义位置
1. 在&#x200B;__内容__&#x200B;下，选择默认内容，然后选择&#x200B;__更改内容片段__
1. 选择要为此体验提供的导出内容片段，然后选择&#x200B;__完成__
1. 查看内容文本区域中的内容片段选件JSON，这是通过内容片段的预览操作在AEM创作服务中提供的相同JSON。
1. 在左边栏中，添加一个体验，然后选择要提供的其他内容片段选件
1. 选择&#x200B;__下一步__，并根据活动需要配置定位规则
   + 在此示例中，将A/B测试保留为手动的50/50拆分。
1. 选择&#x200B;__下一步__，并完成活动设置
1. 选择&#x200B;__保存并关闭__&#x200B;并为其指定一个有意义的名称
1. 在Adobe Target的“活动”中，从右上角的“不活动/激活/存档”下拉列表中选择&#x200B;__激活__。

现在，可以在AEM Headless应用程序中集成和公开针对`wknd-adventure-promo`位置的Adobe Target活动。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform数据流Id{#datastream-id}

AEM Headless应用程序需要[Adobe Experience Platform数据流](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) ID才能使用[Adobe Target Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)与Adobe交互。

+++有关分步说明，请展开

1. 导航到[Adobe Experience Cloud](https://experience.adobe.com/)
1. 打开&#x200B;__Experience Platform__
1. 选择&#x200B;__数据收集>数据流__&#x200B;并选择&#x200B;__新建数据流__
1. 在“新建数据流”向导中，输入：
   + 名称：`AEM Target integration`
   + 描述： `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件架构： `Leave blank`
1. 选择&#x200B;__保存__
1. 选择&#x200B;__添加服务__
1. 在&#x200B;__服务__&#x200B;中选择&#x200B;__Adobe Target__
   + 已启用：__是__
   + 属性令牌： __留空__
   + 目标环境ID： __留空__
      + 可在Adobe Target中的&#x200B;__管理>主机__&#x200B;处设置Target环境。
   + 目标第三方ID命名空间： __留空__
1. 选择&#x200B;__保存__
1. 在右侧，复制&#x200B;__数据流ID__&#x200B;以用于[Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)配置调用。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 向AEM Headless应用程序添加个性化设置{#code}

本教程探讨如何通过[Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)，使用Adobe Target中的内容片段选件对简单的React应用程序进行个性化。 此方法可用于个性化任何基于JavaScript的Web体验。

Android™和iOS移动体验可以使用[Adobe的移动SDK](https://developer.adobe.com/client-sdks/documentation/)按照类似的模式进行个性化。

### 先决条件

+ Node.js 14
+ Git
+ [WKND共享了2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)安装在AEM as a Cloud创作和发布服务上

### 设置

1. 从[Github.com](https://github.com/adobe/aem-guides-wknd-graphql)下载示例React应用程序的源代码

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在收藏的IDE中打开位于`~/Code/aem-guides-wknd-graphql/personalization-tutorial`的代码库
1. 更新您希望应用连接到`~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`的AEM服务主机

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 运行应用程序，并确保该应用程序连接到配置的AEM服务。 在命令行中，执行：

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 将[Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package)安装为NPM包。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   可以在代码中使用Web SDK来按活动位置获取内容片段选件JSON。

   配置Web SDK时，需要两个ID：

   + `edgeConfigId`，即[数据流ID](#datastream-id)
   + `orgId`可在&#x200B;__AEM as a Cloud Service >配置文件>帐户信息>当前组织ID__&#x200B;中找到的Experience Cloud/Target Adobe组织ID

   调用Web SDK时，必须将Adobe Target活动位置（在我们的示例中，`wknd-adventure-promo`）设置为`decisionScopes`数组中的值。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 实施

1. 创建React组件`AdobeTargetActivity.js`以显示Adobe Target活动。

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   使用以下方式调用AdobeTargetActivity React组件：

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. 创建React组件`AdventurePromo.js`以呈现JSON Adobe Target提供的冒险。

   此React组件采用完全水合的JSON，表示冒险内容片段，并以促销方式显示。 根据导出到Adobe Target的内容片段，显示从Adobe Target内容片段选件提供的JSON服务的React组件可以视需要而有所不同，也非常复杂。

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   按如下方式调用此React组件：

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. 将AdobeTargetActivity组件添加到React应用程序的`Home.js`冒险列表上方。

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. 如果React应用未运行，请使用`npm run start`重新启动。

   在两种不同的浏览器中打开React应用程序，以便允许A/B测试向每个浏览器提供不同的体验。 如果两个浏览器显示相同的冒险选件，请尝试关闭/重新打开其中一个浏览器，直到显示另一个体验。

   下图显示了`wknd-adventure-promo`活动基于Adobe Target逻辑显示的两个不同的内容片段选件。

   ![体验选件](./assets/target/offers-in-app.png)

## 恭喜！

现在，我们已将AEM as a Cloud Service配置为将内容片段导出到Adobe Target，在Adobe Target活动中使用了内容片段选件，并在AEM Headless应用程序中显示该活动，从而个性化了体验。
