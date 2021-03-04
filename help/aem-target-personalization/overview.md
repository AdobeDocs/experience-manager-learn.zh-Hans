---
title: AEM 和 Adobe Target 使用入门
seo-title: AEM 和 Adobe Target 使用入门
description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager和Adobe Target创建和提供个性化体验。 在本教程中，您还将了解端到端流程中涉及的不同角色以及他们如何相互协作
seo-description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager和Adobe Target创建和提供个性化体验。 在本教程中，您还将了解端到端流程中涉及的不同角色以及他们如何相互协作
feature: 体验片段
topic: 个性化
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 2%

---


# AEM 和 Adobe Target 使用入门 {#getting-started-with-aem-target}

AEM和目标都是功能重叠且功能强大的解决方案。 客户有时难以理解如何以及何时结合使用这些产品来提供个性化体验。 为了为每个最终用户提供优化的体验，您组织内的不同团队应密切协作并定义谁负责。

在本教程中，我们将介绍AEM和目标的三种不同场景，帮助您了解哪些内容最适合您的组织以及不同团队之间的协作方式。

* 方案1:使用AEM Experience Fragments实现个性化
* 方案2:使用Visual Experience Composer实现个性化
* 方案3:全网页体验的个性化

## 使用AEM Experience Fragments {#personalization-using-aem-experience-fragment}实现个性化

对于此方案，我们将使用AEM和目标。 显然，这两款产品都有各自的优势，在向您网站的用户提供个性化体验时，您需要&#x200B;**个性化内容(来自AEM的内容)**&#x200B;和&#x200B;**智能方式(目标)**&#x200B;才能根据特定用户提供这些内容。

AEM可帮助您创建个性化内容，将您的所有内容和资产集中在一个中心位置，以助您制定个性化战略。 借助AEM，您无需编写代码，即可在一个位置轻松创建适合台式机、平板电脑和移动设备的内容。 无需为每个设备创建页面 — AEM使用您的内容自动调整每个体验。 您还可以通过按钮将内容从AEM导出到Adobe Target，作为优惠。

现在，我们以AEM优惠形式在目标提供个性化内容。 目标允许您根据基于规则的机器学习方法和结合了行为、情境和离线变量的人工智能驱动机器学习方法的组合大规模交付这些优惠。  借助目标，您可以轻松设置和运行A/B和多变量(MVT)活动，以确定最佳优惠、内容和体验。

**体验** 碎片是将内容/体验创造者与使用目标推动业务成果的个性化专业人士联系在一起的一大步。

* AEM内容编辑器作者将个性化内容作为Experience Fragments及其变量
* AEM将Experience Fragment HTML导出到目标 &#x200B;
* 目标&#x200B;使用AEM Experience Fragment标记作为活动中的优惠
* 目标提供Experience Fragment HTML， AEM提供引用的图像

   ![使用体验片段图实现个性化](assets/personalization-use-case-1/use-case-1-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [AEM和Adobe Target(使用旧版Cloud Services)](./implementation.md#integrating-aem-target-options)

***在实施上述集成之后，我们将详细 [探索该方案](./personalization-use-case-1.md)。***

## 使用Visual Experience Composer实现个性化

营销人员可以在其网站上进行快速更改，而无需更改任何代码以使用Adobe Target Visual Experience Composer(VEC)运行测试。 VEC是WYSIWYG（您看到的是您获得的）用户界面，它允许您在站点环境中轻松创建和测试个性化体验和优惠。 您可以通过拖放、交换和修改网页(或优惠)或移动网页的布局和内容，为目标活动创建体验和优惠。

VEC是Adobe Target的主要功能之一。 VEC允许营销人员和设计人员使用可视界面创建和更改内容。 无需直接编辑代码即可做出许多设计选择。 也可以使用编辑器中提供的编辑选项编辑HTML和JavaScript。

* 内容驻留在AEM中，内容编辑器创建和管理站点页面
* 目标使用AEM托管网页运行测试和个性化
* 目标提供个性化内容
* 使用Adobe Target VEC创建新内容
* 适用于AEM托管站点和非AEM托管站点

   ![使用可视体验书写器图表实现个性化](assets/personalization-use-case-3/use-case-diagram-3.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***在实施上述集成之后，我们将详细 [了解该方案。](./personalization-use-case-3.md)***

## 全网页体验的个性化

将Adobe Experience Manager与Adobe Target集成可帮助您为网站用户提供个性化体验。 此外，它还可以帮助您更好地了解哪些版本的网站内容在指定的测试期内可以最佳地提高转化率。 例如，A/B测试会比较您网站内容的两个或多个版本，以查看哪个版本最能提升您的转化率、销售额或您识别的其他指标。 营销人员可以在Adobe Target中创建活动，了解用户如何与网站内容交互以及内容如何影响网站指标。

* 内容驻留在AEM中，内容编辑器创建和管理站点页面
* 目标使用AEM托管网页运行测试和个性化
* 目标提供个性化内容
* 此处未创建新内容
* 适用于AEM和非AEM站点

   ![图](assets/personalization-use-case-2/use-case-2-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***在实施上述集成之后，我们将详细 [了解该方案。](./personalization-use-case-2.md)***
