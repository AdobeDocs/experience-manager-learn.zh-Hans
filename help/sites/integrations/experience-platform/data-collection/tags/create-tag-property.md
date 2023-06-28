---
title: 创建标记属性
description: 了解如何使用最低裸机配置创建标记属性以与AEM集成。 向用户介绍了标记UI，让他们了解扩展、规则和发布工作流程。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 创建标记属性 {#create-tag-property}

了解如何使用最低裸机配置创建标记属性以与Adobe Experience Manager集成。 向用户介绍了标记UI，让他们了解扩展、规则和发布工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 标记属性创建

要创建Tag属性，请完成以下步骤。

1. 在浏览器中，导航到 [Adobe Experience Cloud主页](https://experience.adobe.com/) 页面并使用Adobe ID登录。

1. 单击 **数据收集** 应用程序来自 _快速访问_ Adobe Experience Cloud部分。

1. 单击 **标记** 左侧导航中的菜单项，然后单击 **新建属性** 从右上角。

1. 使用命名标记资产 **名称** 必填字段。 在“域”字段中，输入您的域名；如果使用AEMas a Cloud Service环境，请输入 `adobeaemcloud.com` 并单击 **保存**.

   ![标记属性](assets/tag-properties.png)

## 创建新规则

通过单击以下位置中新创建的Tag属性的名称，打开该属性： **标记属性** 视图。 也位于 _我最近的活动_ 您应会看到标题中添加了核心扩展。 核心标记扩展是默认的扩展，它提供基础事件类型，例如页面加载、浏览器、表单和其他事件类型，请参阅 [核心扩展概述](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 了解更多信息。

规则允许您指定当访客与您的AEM网站交互时应发生的情况。 为了简单起见，让我们将两条消息记录到浏览器控制台中，以演示数据收集标记集成如何能够将JavaScript代码插入AEM网站而不更新AEM项目代码。

要创建规则，请完成以下步骤。

1. 单击 **规则** 从 _创作_ 部分，然后单击 **创建新规则**

1. 使用命名规则 **名称** 必填字段。

1. 单击 **添加** 从 _活动_ 部分，然后在 _事件配置_ 表单，在 **事件类型** 下拉列表选择 _Library Loaded (Page Top)_ 选项并单击 **保留更改**.

1. 单击 **添加** 从 _操作_ 部分，然后在 _操作配置_ 表单，在 **操作类型** 下拉列表选择 _自定义代码_ 选项并单击 **打开编辑器**.

1. 在 _编辑代码_ 模式窗口中，输入以下JavaScript代码片段，然后单击 **保存**，最后点击 **保留更改**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 单击 **保存** 以完成规则创建过程。

   ![新建规则](assets/new-rule.png)

## 添加库并进行发布

标记属性 _规则_ 使用库激活时，请将库视为包含JavaScript代码的包。 按照以下步骤激活新创建的规则。

1. 单击 **发布流** 从 _发布_ 部分，然后单击 **添加库**

1. 使用为您的库命名 **名称** 字段并选择 _开发（开发）_ 选项 **环境** 下拉菜单。

1. 要选择自标记属性创建以来所有更改的资源，请单击 **+添加所有更改的资源**. 此操作会将新创建的规则和核心扩展资源添加到库。 最后点击 **保存并构建到开发环境**.

1. 为构建库后 **开发** 泳道，使用 _椭圆_ 选择 **提交以供审批**

1. 然后在 **已提交** 泳道使用 _椭圆_ 选择 **批准以发布**，同样 **构建并发布到生产环境** 在 **已批准** 泳道。

![已发布库](assets/published-library.png)


上述步骤完成了简单的标记属性创建，该属性创建过程包含一条规则，用于在加载页面时将消息记录到浏览器控制台。 此外，还可通过创建库来发布规则和核心扩展。

## 后续步骤

[使用IMS连接AEM与标记属性](connect-aem-tag-property-using-ims.md)


## 其他资源 {#additional-resources}

* [创建标记属性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
