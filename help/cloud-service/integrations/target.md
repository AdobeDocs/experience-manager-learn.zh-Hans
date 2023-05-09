---
title: AEM Headless和Target个性化
description: 本教程探讨如何将AEM内容片段导出到Adobe Target，然后使用AdobeWeb SDK将无头体验个性化。
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
source-git-commit: b3cc9c4fbd36cdf5be46e4546a174fea0c8da05c
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 1%

---

# 使用内容片段个性化AEM无头体验

>[!IMPORTANT]
>
> Adobe Experience Manager内容片段导出到Adobe Target的功能在AEMas a Cloud Service中可用 [预发行渠道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=zh-Hans#new-features).



本教程探讨如何将AEM内容片段导出到Adobe Target，然后使用AdobeWeb SDK将无头体验个性化。 的 [React WKND应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) 用于探索如何将使用内容片段选件的个性化Target活动添加到体验中，以推广WKND冒险。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教程涵盖设置AEM和Adobe Target时涉及的步骤：

1. [为Adobe Target创建Adobe IMS配置](#adobe-ims-configuration) 在AEM作者中
1. [创建Adobe TargetCloud Service](#adobe-target-cloud-service) 在AEM作者中
1. [将Adobe TargetCloud Service应用于AEM Assets文件夹](#configure-asset-folders) 在AEM作者中
1. [权限Adobe TargetCloud Service](#permission) 在Adobe Admin Console
1. [导出内容片段](#export-content-fragments) 从AEM创作到Target
1. [使用内容片段选件创建活动](#activity) 在Adobe Target
1. [创建Experience Platform数据流](#datastream-id) Experience Platform
1. [将个性化集成到基于React的AEM Headless应用程序中](#code) 使用AdobeWeb SDK。

## Adobe IMS配置{#adobe-ims-configuration}

一种Adobe IMS配置，可帮助在AEM和Adobe Target之间进行身份验证。

审阅 [文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) ，以了解有关如何创建Adobe IMS配置的分步说明。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

在AEM中创建Adobe TargetCloud Service，以便于将内容片段导出到Adobe Target。

审阅 [文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) 有关如何创建Adobe TargetCloud Service的分步说明。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 配置资产文件夹{#configure-asset-folders}

在上下文感知配置中配置的Adobe TargetCloud Service必须应用于包含内容片段的AEM Assets文件夹层次结构，才能导出到Adobe Target。

+++展开以了解分步说明

1. 登录到 __AEM创作服务__ 作为DAM管理员
1. 导航到 __资产>文件__，找到具有 `/conf` 应用于
1. 选择资产文件夹，然后选择 __属性__ 从顶部操作栏
1. 选择 __Cloud Services__ 选项卡
1. 确保将云配置设置为上下文感知配置(`/conf`)，其中包含Adobe TargetCloud Services配置。
1. 选择 __Adobe Target__ 从 __Cloud Service配置__ 下拉列表。
1. 选择 __保存并关闭__ 在右上方

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## 对AEM Target集成授予权限{#permission}

必须授予Adobe Target集成（以developer.adobe.com项目的形式显示） __编辑器__ 产品角色，以便将内容片段导出到Adobe Admin Console。

+++展开以了解分步说明

1. 以可以在Adobe Admin Console中管理Adobe Target产品的用户身份登录Experience Cloud
1. 打开 [Adobe Admin Console](https://adminconsole.adobe.com)
1. 选择 __产品__ 然后打开 __Adobe Target__
1. 在 __产品配置文件__ 选项卡，选择 __*DefaultWorkspace*__
1. 选择 __API凭据__ 选项卡
1. 在此列表中找到您的developer.adobe.com应用程序，并设置其 __产品角色__ to __编辑器__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 将内容片段导出到Target{#export-content-fragments}

位于 [已配置AEM Assets文件夹层次结构](#apply-adobe-target-cloud-service-to-aem-assets-folders) 可作为内容片段选件导出到Adobe Target。 这些内容片段选件是Target中一种特殊形式的JSON选件，可在Target活动中使用，在无头应用程序中提供个性化体验。

+++展开以了解分步说明

1. 登录到 __AEM作者__ 作为DAM用户
1. 导航到 __资产>文件__，并在“已启用Adobe Target”文件夹下找到要导出为JSON的内容片段以将其导出到Target
1. 选择要导出到Adobe Target的内容片段
1. 选择 __导出到Adobe Target选件__ 从顶部操作栏
   + 此操作可将内容片段的完全补充的JSON表示形式导出到Adobe Target作为“内容片段选件”
   + 在AEM中可以回顾充分补充的JSON表示形式
      + 选择内容片段
      + 展开侧面板
      + 选择 __预览__ 图标
      + 导出到Adobe Target的JSON表示形式显示在主视图中
1. 登录到 [Adobe Experience Cloud](https://experience.adobe.com) 具有用户在Adobe Target的编辑者角色
1. 从 [Experience Cloud](https://experience.adobe.com)，选择 __Target__ 从右上方的产品切换器打开Adobe Target。
1. 确保在 __工作区切换器__ 在右上方。
1. 选择 __选件__ 选项卡
1. 选择 __类型__ 下拉菜单，然后选择 __内容片段__
1. 验证从AEM导出的内容片段是否显示在列表中
   + 将鼠标悬停在选件上，然后选择 __查看__ 按钮
   + 查看 __选件信息__ 并查看 __AEM深层链接__ 可直接在AEM创作服务中打开内容片段的

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用内容片段选件定位活动{#activity}

在Adobe Target中，可以创建一个活动，将内容片段选件JSON用作内容，从而允许在无头应用程序中通过在AEM中创建和管理的内容进行个性化体验。

在此示例中，我们使用简单的A/B活动，但可以使用任何Target活动。

+++展开以了解分步说明

1. 选择 __活动__ 选项卡
1. 选择 __+创建活动__，然后选择要创建的活动类型。
   + 此示例创建一个简单 __A/B测试__ 但内容片段选件可以为任何活动类型提供支持
1. 在 __创建活动__ 向导
   + 选择 __Web__
   + 在 __选择体验编辑器__，选择 __表单__
   + 在 __选择工作区__，选择 __默认工作区__
   + 在 __选择属性__，选择活动的可用属性，或选择 __无资产限制__ 以允许在所有属性中使用。
   + 选择 __下一个__ 创建活动
1. 通过选择 __重命名__ 在左上角
   + 为活动提供一个有意义的名称
1. 在初始体验中，设置 __位置1__ ，以便定位活动
   + 在此示例中，定位名为的自定义位置 `wknd-adventure-promo`
1. 在 __内容__ 选择默认内容，然后选择 __更改内容片段__
1. 选择要为此体验提供的导出内容片段，然后选择 __完成__
1. 在内容文本区域中查看内容片段选件JSON，这与AEM创作服务中通过内容片段的预览操作提供的JSON相同。
1. 在左边栏中，添加体验，然后选择要提供的其他内容片段选件
1. 选择 __下一个__，并根据活动需要配置定位规则
   + 在本例中，将A/B测试保留为手动50/50拆分。
1. 选择 __下一个__，并完成活动设置
1. 选择 __保存并关闭__ 给它一个有意义的名字
1. 在Adobe Target的活动中，选择 __激活__ 从右上方的不活动/激活/存档下拉列表中。

定向的Adobe Target活动 `wknd-adventure-promo` 现在，可以在AEM Headless应用程序中集成和显示位置。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform数据流ID{#datastream-id}

安 [Adobe Experience Platform Datastream](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEM Headless应用程序需要使用ID与Adobe Target进行交互 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++展开以了解分步说明

1. 导航到 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 打开 __Experience Platform__
1. 选择 __数据收集>数据流__ 选择 __新数据流__
1. 在“新建数据流”向导中，输入：
   + 名称: `AEM Target integration`
   + 描述: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件架构： `Leave blank`
1. 选择 __保存__
1. 选择 __添加服务__
1. 在 __服务__ 选择 __Adobe Target__
   + 已启用： __是__
   + 资产令牌： __留空__
   + Target环境ID: __留空__
      + 可以在Adobe Target中设置Target环境，网址为 __管理>主机__.
   + Target第三方ID命名空间： __留空__
1. 选择 __保存__
1. 在右侧，复制 __数据流ID__ 用于 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 配置调用。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 将个性化添加到AEM Headless应用程序{#code}

本教程探讨如何通过以下方式使用Adobe Target中的内容片段选件对简单的React应用程序进行个性化 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此方法可用于个性化任何基于JavaScript的Web体验。

Android™和iOS移动体验可以按照类似的模式使用 [Adobe的Mobile SDK](https://developer.adobe.com/client-sdks/documentation/).

### 前提条件

+ Node.js 14
+ Git
+ [WKND共享2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 安装在AEM as a Cloud Author和Publish服务上

### 设置

1. 从下载示例React应用程序的源代码 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开代码库： `~/Code/aem-guides-wknd-graphql/personalization-tutorial` 在您最喜爱的IDE中
1. 更新您希望应用程序连接到的AEM服务主机 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 运行应用程序，并确保它连接到配置的AEM服务。 从命令行中，执行：

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 安装 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) 作为NPM包。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK可以在代码中使用，以按活动位置获取内容片段选件JSON。

   配置Web SDK时，需要两个ID:

   + `edgeConfigId` 是 [数据流ID](#datastream-id)
   + `orgId` AEMas a Cloud Service/TargetAdobe组织ID，可在 __Experience Cloud>配置文件>帐户信息>当前组织ID__

   调用Web SDK时，Adobe Target活动位置(在我们的示例中， `wknd-adventure-promo`)必须设置为 `decisionScopes` 数组。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 实施

1. 创建React组件 `AdobeTargetActivity.js` 以展示Adobe Target活动。

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

   AdobeTargetActivity React组件通过以下方式进行调用：

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. 创建React组件 `AdventurePromo.js` 以渲染冒险JSON Adobe Target提供的内容。

   此React组件采用完全水合的JSON（表示冒险内容片段），并以促销方式显示。 根据导出到Adobe Target的内容片段，显示从Adobe Target内容片段选件提供的JSON的React组件可能会根据需要多种多样且复杂。

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

   此React组件将按如下方式调用：

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. 将AdobeTargetActivity组件添加到React应用程序的 `Home.js` 在历险名单上。

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

1. 如果React应用程序未运行，请重新使用 `npm run start`.

   在两个不同的浏览器中打开React应用程序，以便A/B测试能够向每个浏览器提供不同的体验。 如果两个浏览器显示相同的冒险选件，请尝试关闭/重新打开其中一个浏览器，直到显示其他体验。

   下图显示了 `wknd-adventure-promo` 活动，基于Adobe Target的逻辑。

   ![体验选件](./assets/target/offers-in-app.png)

## 恭喜！

现在，我们已将AEM as a Cloud Service配置为将内容片段导出到Adobe Target，在Adobe Target活动中使用内容片段选件，并在AEM无头应用程序中显现该活动，从而对体验进行个性化。
