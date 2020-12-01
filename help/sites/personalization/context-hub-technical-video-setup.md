---
title: 设置ContextHub以通过AEM Sites实现个性化
description: ContextHub是存储、处理和呈现上下文数据的框架。 ContextHub Javascript API允许您根据需要访问存储以创建、更新和删除数据。 因此，ContextHub表示页面上的数据层。 本页介绍如何将Context Hub添加到AEM站点页面。
feature: context-hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 5%

---


# 为个性化设置ContextHub {#set-up-contexthub}

ContextHub是存储、处理和呈现上下文数据的框架。 ContextHub Javascript API允许您根据需要访问存储以创建、更新和删除数据。 因此，ContextHub表示页面上的数据层。 本页介绍如何将Context Hub添加到AEM站点页面。

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>我们使用此视频的WKND参考站点，它不是AEM版本的一部分。 您可以在此处](https://github.com/adobe/aem-guides-wknd/releases)下载[最新版本。

将ContextHub添加到您的页面以启用ContextHub功能并链接到ContextHub JavaScript库。 ContextHub JavaScript API提供对ContextHub管理的上下文数据的访问。

## 将ContextHub添加到页面组件{#adding-contexthub-to-a-page-component}

要启用ContextHub功能并链接到ContextHub JavaScript库，请在网页的`<head>`部分包含`contexthub`组件。 页面组件的HTL代码类似于以下示例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## 站点配置和ContextHub区段{#site-configuration-and-contexthub-segments}

ContextHub包含一个分段引擎，它管理区段并确定为当前上下文解析哪些区段。 定义了多个区段。 您可以使用Javascript API确定已解析的段[。 ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)在[[!UICONTROL 配置浏览器]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)下为您的站点启用ContextHub区段。

## 创建区段{#create-segments}

创建AEM区段，充当Teaser的规则。 即，它们定义Teaser中的内容何时出现在网页上。 内容可以特别针对于访客的需求和兴趣，具体取决于他们匹配的区段。

## 将云配置、区段路径和ContextHub路径分配给您的站点{#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

将云配置路径、分段路径和ContextHub路径分配给您的站点根节点，以便您为受众创建个性化体验。 使用ContextHub，您可以处理上下文数据并测试已解析的区段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以阅读以下关于ContextHub和分段的更多信息：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [将Context Hub添加到页面和访问商店](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解分段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用ContextHub配置分段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
