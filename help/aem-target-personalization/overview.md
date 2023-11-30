---
title: AEM 和 Adobe Target 使用入门
description: 一个端到端教程，其中演示了如何使用Adobe Experience Manager和Adobe Target创建和提供个性化体验。 在本教程中，您还将了解端到端流程中涉及的不同角色，以及他们如何相互协作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 0%

---

# 集成AEM Sites和Adobe Target {#getting-started-with-aem-target}

AEM和Target都是功能强大的解决方案，但看上去似乎有一些功能重叠。 客户有时难以理解如何以及何时结合使用这些产品来提供个性化体验。 为了向每位最终用户提供优化的体验，贵组织内的不同团队应密切合作，并明确各自的职责。

在本教程中，我们将介绍AEM和Target的三种不同方案，这有助于您了解哪些方案最适合您的组织以及不同团队如何协作。

* 场景1 ：使用AEM Experience Fragments进行个性化
* 场景2 ：使用可视化体验编辑器进行个性化
* 场景3 ：完整网页体验的个性化

## 使用AEM Experience Fragments进行个性化 {#personalization-using-aem-experience-fragment}

对于这种情况，我们将使用AEM和Target。 很显然，这两种产品都有各自的优势，而且在为网站用户提供个性化体验方面，您需要 **个性化内容(AEM内容)** 和 **智能方式(Target)** 根据特定用户提供这些内容。

AEM可帮助您创建个性化内容，将您的所有内容和资产集中到一个中心位置，为您的个性化策略提供动力。 通过AEM，您可以在一个位置轻松创建台式机、平板电脑和移动设备的内容，而无需编写代码。 无需为每个设备创建页面，AEM会自动使用您的内容来调整每个体验。 您还可以通过推送按钮将内容从AEM导出到Adobe Target作为选件。

现在，我们在Target中提供了来自AEM的优惠形式的个性化内容。 通过Target，您可以根据包含行为、上下文和离线变量的基于规则的机器学习方法和人工智能驱动的机器学习方法的组合，大规模提供这些选件。  通过Target，您可以轻松地设置并运行A/B活动和多变量(MVT)活动，从而确定最佳的选件、内容和体验。

**体验片段** 在将内容/体验创建者与使用Target推动业务成果的个性化专业人员联系起来方面，迈出了一大步。

* AEM内容编辑器作者将内容个性化为体验片段及其变量
* AEM将体验片段HTML导出到Target&#x200B;。
* Target&#x200B;使用AEM体验片段标记作为活动中的选件
* Target提供体验片段HTML，AEM提供引用的图像

  ![使用体验片段图进行个性化](assets/personalization-use-case-1/use-case-1-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [使用旧版Cloud Service的AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来探索 [详细情景](./personalization-use-case-1.md).***

## 使用可视化体验编辑器进行个性化

营销人员可以对其网站进行快速更改，而无需更改任何代码以使用Adobe Target可视化体验编辑器(VEC)运行测试。 VEC是WYSIWYG（所见即所得）用户界面，可让您轻松地在站点上下文中创建和测试个性化体验和选件。 您可以通过拖放、交换和修改网页（或选件）或移动网页的布局和内容来为Target活动创建体验和选件。

VEC是Adobe Target的主要功能之一。 通过VEC，营销人员和设计人员可以使用可视化界面创建和更改内容。 无需直接编辑代码，即可做出许多设计选择。 也可以使用编辑器中提供的编辑选项来编辑HTML和JavaScript。

* 内容驻留在AEM中，内容编辑者可创建和管理站点页面
* Target使用AEM托管的网站页面来运行测试和个性化
* Target提供个性化内容
* 使用Adobe Target VEC创建全新内容
* 适用于AEM托管站点和非AEM托管站点

  ![使用可视化体验编辑器图进行个性化](assets/personalization-use-case-3/use-case-diagram-3.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来探索 [场景详细信息。](./personalization-use-case-3.md)***

## 完整网页体验的个性化

将Adobe Experience Manager与Adobe Target集成可帮助您为网站用户提供个性化体验。 此外，它还有助于您更好地了解在指定的测试期间，哪些网站内容版本最能改善转化。 例如，A/B测试可比较两个或更多版本的网站内容，以了解哪些版本最有利于提高您的转化率、销售额或您识别的其他指标。 营销人员可以在Adobe Target中创建活动，以了解用户如何与网站内容进行交互以及它如何影响您的网站量度。

* 内容驻留在AEM中，内容编辑者可创建和管理站点页面
* Target使用AEM托管的网站页面来运行测试和个性化
* Target提供个性化内容
* 此处不创建任何全新内容
* 适用于AEM和非AEM站点

  ![图](assets/personalization-use-case-2/use-case-2-diagram.png)

**要实施此方案，您需要：**

* [使用Launch和Adobe I/O集成AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来探索 [场景详细信息。](./personalization-use-case-2.md)***
