---
title: AEM 和 Adobe Target 使用入门
description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager和Adobe Target创建和提供个性化体验。 在本教程中，您还将了解端到端流程中涉及的不同角色，以及他们如何相互协作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# AEM 和 Adobe Target 使用入门 {#getting-started-with-aem-target}

AEM和Target都是功能强大的解决方案，而且看似功能重叠。 客户有时难以理解如何以及何时将这些产品与提供个性化体验结合使用。 为了为每个最终用户提供优化的体验，您组织内的不同团队应密切合作，并定义谁来做什么。

在本教程中，我们将介绍AEM和Target的三种不同情景，帮助您了解哪些情景最适合您的组织，以及不同团队的协作方式。

* 场景1 :使用AEM体验片段进行个性化
* 场景2 :使用可视化体验编辑器进行个性化
* 场景3 :完整网页体验的个性化

## 使用AEM体验片段进行个性化 {#personalization-using-aem-experience-fragment}

对于此方案，我们将使用AEM和Target。 显然，这两种产品都有各自的优势，在向网站用户提供个性化体验时，您需要 **个性化内容(AEM中的内容)** 和 **智能方式(Target)** 以根据特定用户提供这些内容。

AEM可帮助您创建个性化内容，将所有内容和资产汇集到一个中心位置，以助您制定个性化策略。 通过AEM，您可以在一个位置轻松地为台式机、平板电脑和移动设备创建内容，而无需编写代码。 无需为每个设备创建页面 — AEM可使用您的内容自动调整每个体验。 您还可以通过按钮将内容从AEM导出为选件。

现在，我们在Target中以“AEM提供的选件”的形式提供了个性化内容。 Target让您能够根据基于规则和AI驱动的机器学习方法的组合大规模交付这些选件，这些方法包含行为、上下文和离线变量。  借助Target，您可以轻松设置并运行A/B和多变量(MVT)活动，以确定最佳的选件、内容和体验。

**体验片段** 在将内容/体验创建器与使用Target推动业务成果的个性化专业人士关联方面，这是向前迈出的一大步。

* AEM内容编辑器作者将个性化内容作为体验片段及其变量
* AEM导出Experience FragmentHTML到Target &#x200B;
* Target使&#x200B;用AEM体验片段标记作为活动中的选件
* Target提供体验片段HTML,AEM提供引用的图像

   ![使用体验片段图进行个性化](assets/personalization-use-case-1/use-case-1-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [AEM和Adobe Target(使用旧版Cloud Services)](./implementation.md#integrating-aem-target-options)

***实施上述集成后，我们可以 [方案详细信息](./personalization-use-case-1.md).***

## 使用可视化体验编辑器进行个性化

营销人员无需更改任何代码即可在其网站上快速更改以使用Adobe Target可视化体验编辑器(VEC)运行测试。 VEC是WYSIWYG（您所看到的是您获得的内容）用户界面，可让您轻松地在站点环境中创建和测试个性化体验和选件。 您可以通过拖放、交换和修改网页（或选件）或移动网页的布局和内容，为Target活动创建体验和选件。

VEC是Adobe Target的主要功能之一。 通过VEC，营销人员和设计人员可以使用可视化界面创建和更改内容。 无需直接编辑代码，即可做出许多设计选择。 使用编辑器中提供的编辑选项，也可以编辑HTML和JavaScript。

* 内容驻留在AEM中，内容编辑器创建和管理网站页面
* Target使用AEM托管的网站页面运行测试和个性化
* Target提供个性化内容
* 使用Adobe Target VEC创建新内容
* 适用于AEM托管网站和非AEM托管网站

   ![使用可视化体验编辑器进行个性化的图表](assets/personalization-use-case-3/use-case-diagram-3.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***在实施上述集成后，我们可以浏览 [方案。](./personalization-use-case-3.md)***

## 完整网页体验的个性化

将Adobe Experience Manager与Adobe Target集成可帮助您为网站用户提供个性化体验。 此外，它还有助于您更好地了解哪些版本的网站内容在指定的测试期间最能提高转化。 例如，A/B测试会比较网站内容的两个或多个版本，以查看哪个版本最能提升转化率、销售额或您识别的其他量度。 营销人员可以在Adobe Target中创建活动，以了解用户与网站内容的交互方式以及该活动对网站量度有何影响。

* 内容驻留在AEM中，内容编辑器创建和管理网站页面
* Target使用AEM托管的网站页面运行测试和个性化
* Target提供个性化内容
* 此处未创建新内容
* 适用于AEM和非AEM网站

   ![图表](assets/personalization-use-case-2/use-case-2-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***在实施上述集成后，我们可以浏览 [方案。](./personalization-use-case-2.md)***
