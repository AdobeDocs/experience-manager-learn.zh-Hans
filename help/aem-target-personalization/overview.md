---
title: AEM 和 Adobe Target 快速入门
description: 一个端到端教程，其中演示了如何使用 Adobe Experience Manager 和 Adobe Target 创建和投放个性化体验。在本教程中，您还将了解端到端流程中涉及的不同角色以及他们如何相互协作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# 集成 AEM Sites 与 Adobe Target {#getting-started-with-aem-target}

AEM 和 Target 都是功能强大的解决方案，但看上去似乎有一些功能重叠。客户有时难以理解如何以及何时结合使用这两个产品来投放个性化体验。为了给每一位最终用户提供最佳体验，贵组织内的不同团队应密切合作且明确分工。

在本教程中，我们介绍了 AEM 和 Target 的三种不同场景，以帮助您了解最适合您组织的方法以及不同团队如何协作。

* 场景 1：使用 AEM 体验片段进行个性化
* 场景 2：使用可视化体验编辑器进行个性化
* 场景 3：整个网页体验的个性化

## 使用 AEM 体验片段进行个性化 {#personalization-using-aem-experience-fragment}

对于该场景，我们将使用 AEM 和 Target。显然，这两种产品都有各自的优势，当需要为网站用户提供个性化体验时，您需要&#x200B;**个性化内容（来自 AEM 的内容）**&#x200B;和一个&#x200B;**智能方式（Target）**&#x200B;来根据特定用户提供这些内容。

AEM 可助您打造个性化内容，将您的所有内容和资产汇聚于一处，以推动您的个性化策略。借助 AEM，您无需创作代码，即可轻松为台式机、平板电脑和移动设备创作内容。无需为每种设备单独创建页面——AEM 能够使用您的内容自动调整每次体验。您只需轻轻一按，即可将 AEM 中的内容作为产品建议导出至 Adobe Target。

我们现在在 Target 中拥有来自 AEM 的个性化内容，即产品建议信息。Target 允许您基于规则和人工智能驱动的机器学习方法的组合，大规模地提供这些产品建议信息，这些方法综合考虑了行为、情境和离线变量。通过 Target，您可以轻松地设置并运行 A/B 测试和多变量 (MVT) 活动，从而确定最佳的产品建议、内容和体验。

**体验片段**&#x200B;代表着将内容/体验创作者与利用 Target 推动业务成果的个性化专业人士联系起来的重要一步。

* AEM 内容编辑器将个性化内容创作为体验片段及其变体
* AEM 将体验片段 HTML 导出到 Target
* Target 使用 AEM 体验片段标记作为活动中的产品建议
* Target 提供体验片段 HTML，而 AEM 则提供引用图像

  ![使用体验片段图表进行个性化](assets/personalization-use-case-1/use-case-1-diagram.png)

**要实现此场景，您需要：**

* [使用标记和 Adobe I/O 集成 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)
* [使用旧版云服务的 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来详细探索一下该[场景](./personalization-use-case-1.md)。***

## 使用可视化体验编辑器进行个性化

营销人员可以在其网站上进行快速更改，而无需更改任何代码即可使用 Adobe Target Visual Experience Composer (VEC) 运行测试。VEC 是 WYSIWYG（所见即所得）用户界面，可让您轻松地在网站环境中创建和测试个性化体验和产品建议。您可以通过拖放、交换和修改网页（或产品建议）或移动网页的布局和内容来为 Target 活动创建体验和产品建议。

VEC 是 Adobe Target 的主要功能之一。通过 VEC，营销人员和设计人员可以使用可视化界面创建和更改内容。无需直接编辑代码，便可进行多种设计选择。使用编辑器提供的编辑选项也可以编辑 HTML 和 JavaScript。

* 内容存储在 AEM 中，内容编辑人员负责创建和管理网站页面
* Target 利用 AEM 托管的网站页面进行测试和个性化设置
* Target 提供个性化内容
* 使用 Adobe Target VEC 创建全新内容
* 适用于 AEM 托管的网站和非 AEM 托管的网站

  ![使用可视化体验编辑器进行个性化示意图](assets/personalization-use-case-3/use-case-diagram-3.png)

**要实现此场景，您需要：**

* [使用标记和 Adobe I/O 集成 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来详细探索一下该[场景](./personalization-use-case-3.md)***。

## 完整网页体验的个性化

将 Adobe Experience Manager 与 Adobe Target 集成可帮助您为网站用户提供个性化的体验。此外，它还可以帮助您更好地了解在指定的测试期间哪些版本的网站内容最能提高您的转化率。例如，A/B 测试可比较两个或更多版本的网站内容，以了解哪个版本的内容可最大程度地提升转化次数、销售额或您标识的其他量度。营销人员可以在 Adobe Target 中创建活动来了解用户如何与您网站的内容互动以及它如何影响您的网站量度。

* 内容存储在 AEM 中，内容编辑人员负责创建和管理网站页面
* Target 利用 AEM 托管的网站页面进行测试和个性化设置
* Target 提供个性化内容
* 这里没有创建任何新内容
* 适用于 AEM 和非 AEM 网站

  ![示意图](assets/personalization-use-case-2/use-case-2-diagram.png)

**要实现此场景，您需要：**

* [使用标记和 Adobe I/O 集成 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***实施上述集成后，让我们来详细探索一下该[场景](./personalization-use-case-2.md)***。
