---
title: 创建标记属性
description: 了解如何使用最低裸机配置创建标记属性以与AEM集成。 将用户引入标记UI，并了解扩展、规则和发布工作流程。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 创建标记属性 {#create-tag-property}

了解如何使用最低裸机配置创建标记属性以与Adobe Experience Manager集成。 将用户引入标记UI，并了解扩展、规则和发布工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 标记属性创建

要创建标记属性，请完成以下步骤。

1. 在浏览器中，导航到 [Adobe Experience Cloud主页](https://experience.adobe.com/) 页面和登录，使用Adobe ID。

1. 单击 **数据收集** 应用程序 _快速访问_ Adobe Experience Cloud主页的部分。

1. 单击 **标记** 菜单项，然后单击 **新建资产** 从右上角。

1. 使用为您的标记资产命名 **名称** 必填字段。 对于“域”字段，输入您的域名，或者如果使用AEMas a Cloud Service环境，请输入 `adobeaemcloud.com` 单击 **保存**.

   ![标记属性](assets/tag-properties.png)

## 创建新规则

在 **标记属性** 中。 也在 _我最近的活动_ 标题时，您应该会看到核心扩展已添加到该扩展中。 核心标记扩展是默认扩展，它提供了基本事件类型，如页面加载、浏览器、表单和其他事件类型，请参阅 [核心扩展概述](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 以了解更多信息。

规则允许您指定当访客与您的AEM网站交互时应发生的情况。 为了简单起见，让我们将两条消息记录到浏览器控制台，以演示数据收集标记集成如何在不更新AEM项目代码的情况下将JavaScript代码注入您的AEM站点。

要创建规则，请完成以下步骤。

1. 单击 **规则** 从 _创作_ ，然后单击 **创建新规则**

1. 使用命名规则 **名称** 必填字段。

1. 单击 **添加** 从 _事件_ ，然后在 _事件配置_ 表单 **事件类型** 下拉选择 _Library Loaded(Page Top)_ 选项并单击 **保留更改**.

1. 单击 **添加** 从 _操作_ ，然后在 _操作配置_ 表单 **操作类型** 下拉选择 _自定义代码_ 选项并单击 **Open Editor**.

1. 在 _编辑代码_ 模式窗口中，输入以下JavaScript代码段，然后单击 **保存**，最后单击 **保留更改**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 单击 **保存** 完成规则创建过程。

   ![新规则](assets/new-rule.png)

## 添加库并发布它

标记属性 _规则_ 会使用库激活，请将库视为包含JavaScript代码的包。 按照以下步骤激活新创建的规则。

1. 单击 **发布流程** 从 _发布_ ，然后单击 **添加库**

1. 使用为库命名 **名称** 字段和选择 _开发（开发）_ 选项 **环境** 下拉列表。

1. 要选择自创建标记属性以来所有已更改的资源，请单击 **+添加所有已更改的资源**. 此操作会将新创建的规则和核心扩展资源添加到库中。 最后单击 **保存并构建到开发环境**.

1. 在为 **开发** 游泳道，使用 _省略号_ 选择 **提交以供审批**

1. 然后，在 **已提交** 使用游泳道 _省略号_ 选择 **批准发布**&#x200B;同样， **构建并发布到生产环境** 在 **已批准** 游泳道。

![已发布的库](assets/published-library.png)


上述步骤将完成简单的标记属性创建，该属性创建具有在加载页面时将消息记录到浏览器控制台的规则。 此外，还可以通过创建库来发布规则和核心扩展。

## 后续步骤

[使用IMS将AEM与标记属性连接](connect-aem-tag-property-using-ims.md)


## 其他资源 {#additional-resources}

* [创建标记属性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
