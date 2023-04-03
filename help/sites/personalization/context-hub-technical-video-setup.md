---
title: 使用AEM Sites设置ContextHub进行个性化
description: ContextHub是用于存储、处理和呈现上下文数据的框架。 ContextHub Javascript API允许您访问存储区，以根据需要创建、更新和删除数据。 因此， ContextHub表示页面上的数据层。 本页介绍如何将ContextHub添加到AEM网站页面。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 3%

---

# 为个性化设置ContextHub {#set-up-contexthub}

ContextHub是用于存储、处理和呈现上下文数据的框架。 ContextHub Javascript API允许您访问存储区，以根据需要创建、更新和删除数据。 因此， ContextHub表示页面上的数据层。 本页介绍如何将ContextHub添加到AEM网站页面。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>我们为此视频使用WKND引用网站，但该网站未包含在AEM版本中。 您可以下载 [此处提供最新版本](https://github.com/adobe/aem-guides-wknd/releases).

将ContextHub添加到您的页面以启用ContextHub功能并链接到ContextHub JavaScript库。 ContextHub JavaScript API提供对ContextHub管理的上下文数据的访问。

## 将ContextHub添加到页面组件 {#adding-contexthub-to-a-page-component}

要启用ContextHub功能并链接到ContextHub JavaScript库，请包括 `contexthub` 组件 `<head>` 的子代码。 页面组件的HTL代码类似于以下示例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## 网站配置和ContextHub区段 {#site-configuration-and-contexthub-segments}

ContextHub包括一个分段引擎，用于管理区段并确定哪些区段针对当前上下文进行解析。 定义了多个区段。 您可以使用Javascript API [确定已解析的区段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). 在 [[!UICONTROL 配置浏览器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html).

## 创建区段 {#create-segments}

创建可用作Teaser规则的AEM区段。 即，定义Teaser中的内容在网页上显示的时间。 然后，可以根据访客的需求和兴趣明确定位内容，具体取决于他们匹配的区段。

## 将云配置、区段路径和ContextHub路径分配给您的网站 {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

将云配置路径、分段路径和ContextHub路径分配到您的站点根节点，以便能够为受众创建个性化体验。 使用ContextHub，您可以处理上下文数据并测试已解析的区段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以在下面阅读有关ContextHub和分段的更多信息：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [将ContextHub添加到页面和访问存储](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解分段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用 ContextHub 配置分段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
