---
title: 创建标记属性
description: 了解如何使用最低裸机配置创建标记属性以与AEM集成。 向用户介绍了标记UI，让他们了解扩展、规则和发布工作流程。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 创建标记属性 {#create-tag-property}

了解如何使用最低裸机配置创建标记属性以与Adobe Experience Manager集成。 向用户介绍了标记UI，让他们了解扩展、规则和发布工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 标记属性创建

要创建Tag属性，请完成以下步骤。

1. 在浏览器中，导航到[Adobe Experience Cloud主页](https://experience.adobe.com/)页面，然后使用您的Adobe ID登录。

1. 从Adobe Experience Cloud主页的&#x200B;_快速访问_&#x200B;部分中单击&#x200B;**数据收集**&#x200B;应用程序。

1. 单击左侧导航栏中的&#x200B;**标记**&#x200B;菜单项，然后单击右上角的&#x200B;**新建属性**。

1. 使用&#x200B;**Name**&#x200B;必填字段命名您的标记属性。 在“域”字段中，输入您的域名；如果使用AEM as a Cloud Service环境，请输入`adobeaemcloud.com`并单击&#x200B;**保存**。

   ![标记属性](assets/tag-properties.png)

## 创建新规则

在&#x200B;**标记属性**&#x200B;视图中单击新创建的标记属性的名称以打开该属性。 此外，在&#x200B;_我最近使用的活动_&#x200B;标题下，您应该会看到核心扩展已添加到其中。 核心标记扩展是默认的扩展，它提供基础事件类型，如页面加载、浏览器、表单和其他事件类型。有关详细信息，请参阅[核心扩展概述](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=zh-Hans)。

规则允许您指定当访客与您的AEM网站交互时应发生的情况。 为了简单起见，让我们将两条消息记录到浏览器控制台中，以演示数据收集标记集成如何在不更新AEM项目代码的情况下将JavaScript代码插入您的AEM网站。

要创建规则，请完成以下步骤。

1. 从左侧导航栏的&#x200B;_创作_&#x200B;部分单击&#x200B;**规则**，然后单击&#x200B;**创建新规则**

1. 使用&#x200B;**Name**&#x200B;必填字段命名您的规则。

1. 从&#x200B;_EVENTS_&#x200B;部分中单击&#x200B;**添加**，然后在&#x200B;_事件配置_&#x200B;表单的&#x200B;**事件类型**&#x200B;下拉列表中选择&#x200B;_已加载库（页面顶部）_&#x200B;选项，然后单击&#x200B;**保留更改**。

1. 单击&#x200B;_操作_&#x200B;部分中的&#x200B;**添加**，然后在&#x200B;_操作配置_&#x200B;表单的&#x200B;**操作类型**&#x200B;下拉列表中选择&#x200B;_自定义代码_&#x200B;选项，然后单击&#x200B;**打开编辑器**。

1. 在&#x200B;_编辑代码_&#x200B;模式中，输入以下JavaScript代码片段，然后单击&#x200B;**保存**，最后单击&#x200B;**保留更改**。

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 单击&#x200B;**保存**&#x200B;以完成规则创建过程。

   ![新规则](assets/new-rule.png)

## 添加库并进行发布

Tag属性&#x200B;_Rules_&#x200B;是使用库激活的，请将库视为包含JavaScript代码的包。 按照以下步骤激活新创建的规则。

1. 从左侧导航栏的&#x200B;_PUBLISHING_&#x200B;部分中单击&#x200B;**发布流**，然后单击&#x200B;**添加库**

1. 使用&#x200B;**名称**&#x200B;字段命名库，并为&#x200B;**环境**&#x200B;下拉列表中选择&#x200B;_开发（开发）_&#x200B;选项。

1. 要选择自标记属性创建以来所有更改的资源，请单击&#x200B;**+添加所有更改的资源**。 此操作可将新创建的规则和核心扩展资源添加到库。 最后，单击&#x200B;**保存并生成到开发**。

1. 为&#x200B;**开发**&#x200B;泳道构建库后，使用&#x200B;_省略号_&#x200B;选择&#x200B;**提交以供审批**

1. Publish然后在&#x200B;**已提交**&#x200B;使用省略号&#x200B;_的泳道_&#x200B;中，选择&#x200B;**批准发布**，同样地，在&#x200B;**已批准**&#x200B;泳道中&#x200B;**构建并复制到生产环境**。

![已发布的库](assets/published-library.png)


上述步骤将完成简单的标记属性创建，该属性创建程序具有一个规则，可在加载页面时将消息记录到浏览器控制台。 此外，还通过创建库来发布规则和核心扩展。

## 后续步骤

[使用IMS连接AEM与标记属性](connect-aem-tag-property-using-ims.md)


## 其他资源 {#additional-resources}

* [创建标记属性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=zh-Hans)
