---
title: AEM Headless和Target个性化
description: 本教程探讨了如何将AEM内容片段导出到Adobe Target，然后使用AdobeWeb SDK将Headless体验个性化。
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

# 使用内容片段个性化AEM Headless体验

>[!IMPORTANT]
>
> 在AEMas a Cloud Service中提供了Adobe Experience Manager内容片段导出到Adobe Target [预发行渠道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=zh-Hans#new-features).



本教程探讨了如何将AEM内容片段导出到Adobe Target，然后使用AdobeWeb SDK将Headless体验个性化。 此 [React WKND应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) 用于探索如何使用内容片段选件向体验中添加个性化的Target活动，以促进WKND冒险。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教程介绍了设置AEM和Adobe Target所涉及的步骤：

1. [为Adobe Target创建Adobe IMS配置](#adobe-ims-configuration) 在AEM创作中
1. [创建Adobe TargetCloud Service](#adobe-target-cloud-service) 在AEM创作中
1. [将Adobe TargetCloud Service应用于AEM Assets文件夹](#configure-asset-folders) 在AEM创作中
1. [权限Adobe TargetCloud Service](#permission) 在Adobe Admin Console中
1. [导出内容片段](#export-content-fragments) 从AEM Author到Target
1. [使用内容片段选件创建活动](#activity) 在Adobe Target中
1. [创建Experience Platform数据流](#datastream-id) 在Experience Platform中
1. [将个性化集成到基于React的AEM Headless应用程序中](#code) 使用AdobeWeb SDK。

## Adobe IMS配置{#adobe-ims-configuration}

便于在AEM和Adobe Target之间进行身份验证的Adobe IMS配置。

审核 [文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) 有关如何创建Adobe IMS配置的分步说明。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

在AEM中创建Adobe TargetCloud Service，以便于将内容片段导出到Adobe Target。

审核 [文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) 有关如何创建Adobe TargetCloud Service的分步说明。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 配置资源文件夹{#configure-asset-folders}

在上下文感知配置中配置的Adobe TargetCloud Service，必须应用于包含要导出到Adobe Target的AEM Assets文件夹层次结构。

+++有关分步说明，请展开

1. 登录 __AEM作者服务__ 作为DAM管理员
1. 导航到 __资产>文件__，找到具有 `/conf` 应用于
1. 选择资源文件夹，然后选择 __属性__ 从顶部操作栏中
1. 选择 __Cloud Services__ 选项卡
1. 确保将云配置设置为上下文感知配置(`/conf`)，其中包含Adobe Target Cloud Services配置。
1. 选择 __Adobe Target__ 从 __Cloud Service配置__ 下拉菜单。
1. 选择 __保存并关闭__ 右上角

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## 许可AEM Target集成{#permission}

作为developer.adobe.com项目显示的Adobe Target集成必须获得 __编辑者__ Adobe Admin Console产品角色，用于将内容片段导出到Adobe Target。

+++有关分步说明，请展开

1. 以可在Adobe Admin Console中管理Adobe Target产品的用户的身份登录Experience Cloud
1. 打开 [Adobe Admin Console](https://adminconsole.adobe.com)
1. 选择 __产品__ 然后打开 __Adobe Target__
1. 在 __产品配置文件__ 选项卡，选择 __*默认工作区*__
1. 选择 __API凭据__ 选项卡
1. 在此列表中找到您的developer.adobe.com应用程序并设置其 __产品角色__ 到 __编辑者__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 将内容片段导出到目标{#export-content-fragments}

存在于下的内容片段 [已配置AEM Assets文件夹层次结构](#apply-adobe-target-cloud-service-to-aem-assets-folders) 可导出到Adobe Target作为内容片段选件。 这些内容片段选件（Target中特殊形式的JSON选件）可以在Target活动中使用，以在Headless应用程序中提供个性化体验。

+++有关分步说明，请展开

1. 登录 __AEM创作__ 作为DAM用户
1. 导航到 __资产>文件__，并在“已启用Adobe Target”文件夹下找到要导出为JSON到Target的内容片段
1. 选择要导出到Adobe Target的内容片段
1. 选择 __导出到Adobe Target选件__ 从顶部操作栏中
   + 此操作将内容片段的完全水合JSON表示形式作为“内容片段选件”导出到Adobe Target
   + 可以在AEM中查看完全水合的JSON表示形式
      + 选择内容片段
      + 展开侧面板
      + 选择 __预览__ 图标图标
      + 导出到Adobe Target的JSON表示形式将显示在主视图中
1. 登录 [Adobe Experience Cloud](https://experience.adobe.com) 具有Adobe Target的“编辑者”角色的用户
1. 从 [Experience Cloud](https://experience.adobe.com)，选择 __Target__ 从右上方的产品切换器中打开Adobe Target。
1. 确保在 __工作区切换器__ 右上角。
1. 选择 __选件__ 选项卡
1. 选择 __类型__ 下拉列表，然后选择 __内容片段__
1. 验证从AEM导出的内容片段是否显示在列表中
   + 将鼠标悬停在选件上，然后选择 __视图__ 按钮
   + 查看 __选件信息__ 并查看 __AEM深层链接__ 以直接在AEM创作服务中打开内容片段

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用内容片段选件的Target活动{#activity}

在Adobe Target中，可以创建使用内容片段选件JSON作为内容的活动，允许在Headless应用程序中通过AEM中创建和管理内容提供个性化体验。

在本例中，我们使用简单的A/B活动，但可以使用任何Target活动。

+++有关分步说明，请展开

1. 选择 __活动__ 选项卡
1. 选择 __+创建活动__，然后选择要创建的活动类型。
   + 此示例创建一个简单的 __A/B测试__ 但内容片段选件可以支持任何活动类型
1. 在 __创建活动__ 向导
   + 选择 __Web__
   + In __选择体验编辑器__，选择 __表单__
   + In __选择工作区__，选择 __默认工作区__
   + In __选择属性__，选择活动可用的属性，或选择 __无资产限制__ 以允许在所有“属性”中使用它。
   + 选择 __下一个__ 创建活动
1. 通过选择重命名活动 __重命名__ 左上角
   + 为活动提供一个有意义的名称
1. 在初始体验中，设置 __位置1__ 针对要定位的活动
   + 在此示例中，定位名为的自定义位置 `wknd-adventure-promo`
1. 下 __内容__ 选择默认内容，然后选择 __更改内容片段__
1. 选择要为此体验提供的导出内容片段，然后选择 __完成__
1. 查看内容文本区域中的内容片段选件JSON，这是通过内容片段的预览操作在AEM创作服务中提供的相同JSON。
1. 在左边栏中，添加一个体验，然后选择要提供的其他内容片段选件
1. 选择 __下一个__，并根据活动需要配置定位规则
   + 在此示例中，将A/B测试保留为手动的50/50拆分。
1. 选择 __下一个__，并完成活动设置
1. 选择 __保存并关闭__ 给它取个有意义的名称
1. 在Adobe Target的活动中，选择 __激活__ 从右上角的非活动/激活/存档下拉菜单中。

定位的Adobe Target活动 `wknd-adventure-promo` 位置现在可以在AEM Headless应用程序中集成和公开。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform的数据流ID{#datastream-id}

An [Adobe Experience Platform数据流](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEM Adobe Target Headless应用程序需要ID才能使用 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++有关分步说明，请展开

1. 导航到 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 打开 __Experience Platform__
1. 选择 __数据收集>数据流__ 并选择 __新建数据流__
1. 在“新建数据流”向导中，输入：
   + 名称: `AEM Target integration`
   + 描述: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件架构： `Leave blank`
1. 选择 __保存__
1. 选择 __添加服务__
1. In __服务__ 选择 __Adobe Target__
   + 已启用： __是__
   + 资产令牌： __留空__
   + 目标环境ID： __留空__
      + 可在Adobe Target中设置目标环境，网址为 __管理>主机__.
   + 目标第三方ID命名空间： __留空__
1. 选择 __保存__
1. 在右侧，复制 __数据流ID__ 在中使用 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 配置调用。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 向AEM Headless应用程序添加个性化设置{#code}

本教程探讨了如何在Adobe Target中使用“内容片段选件”，通过以下方式个性化一个简单的React应用程序 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此方法可用于个性化任何基于JavaScript的Web体验。

Android™和iOS移动体验可以使用以下类似模式进行个性化 [Adobe的移动SDK](https://developer.adobe.com/client-sdks/documentation/).

### 前提条件

+ Node.js 14
+ Git
+ [WKND共享2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 安装在AEM as a Cloud创作和发布服务上

### 设置

1. 从下载示例React应用程序的源代码 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开代码库位置 `~/Code/aem-guides-wknd-graphql/personalization-tutorial` 在您最喜爱的IDE中
1. 更新您希望应用程序连接到的AEM服务主机 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 运行应用程序，并确保它连接到配置的AEM服务。 在命令行中，执行：

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

   Web SDK可用于代码中，以按活动位置获取内容片段选件JSON。

   配置Web SDK时，需要两个ID：

   + `edgeConfigId` 即 [数据流ID](#datastream-id)
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

1. 创建React组件 `AdobeTargetActivity.js` 显示Adobe Target活动。

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

1. 创建React组件 `AdventurePromo.js` 来演绎冒险JSON Adobe Target的作品。

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

1. 将AdobeTargetActivity组件添加到React应用程序的 `Home.js` 在冒险清单之上。

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

1. 如果React应用程序未运行，请使用 `npm run start`.

   在两种不同的浏览器中打开React应用程序，以便允许A/B测试向每个浏览器提供不同的体验。 如果两个浏览器显示相同的冒险选件，请尝试关闭/重新打开其中一个浏览器，直到显示另一个体验。

   下图显示了为显示的两个不同的内容片段选件 `wknd-adventure-promo` 活动，基于Adobe Target的逻辑。

   ![体验选件](./assets/target/offers-in-app.png)

## 恭喜！

现在，我们已将AEMas a Cloud Service配置为将内容片段导出到Adobe Target，已在Adobe Target活动中使用内容片段选件，并在AEM Headless应用程序中显示该活动，从而个性化体验。
