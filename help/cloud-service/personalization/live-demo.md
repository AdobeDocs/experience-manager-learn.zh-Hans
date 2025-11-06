---
title: Personalization用例的实时演示
description: 通过A/B测试、行为定位和已知用户个性化示例，在WKND启用网站上实际体验个性化。
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: ed7af09d747d54a84d2583073d3c731388b5f516
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 1%

---

# 个性化用例的实时演示

访问[WKND启用网站](https://wknd.enablementadobe.com/us/en.html){target="wknd"}以查看A/B测试、行为定位和已知用户个性化的实际示例。

>[!VIDEO](https://video.tv.adobe.com/v/3476461/?learn=on&enablevpops)

本页将指导您完成每个个性化方案的实践演示。 在您自己的AEM网站上构建这些功能之前，请使用它来探索可用功能。

>[!IMPORTANT]
>
> 在多个浏览器窗口或无痕浏览/专用浏览模式下打开演示站点，以同时体验不同的个性化变体。
> 使用专用浏览模式时，Firefox和Safari可能会阻止ECID Cookie，或者使用常规浏览模式或在尝试新的个性化方案之前清除Cookie。

## 演示用例

[WKND启用网站](https://wknd.enablementadobe.com/us/en.html){target="wknd"}演示了三种类型的个性化：

| Personalization类型 | 您将会看到的内容 | 时间安排 |
|---------------------|-----------------|---------|
| **行为定位** | 内容会根据您的浏览行为和兴趣进行调整。 通常称为&#x200B;_下一页面或同一页面个性化_ | 实时和批处理 |
| **已知用户Personalization** | 根据从多个系统上的数据构建的完整客户配置文件定制体验。 通常称为大规模的&#x200B;_个性化_ | 实时 |
| **A/B 测试** | 测试不同的内容变体以找到最佳业绩者。 通常称为&#x200B;_试验_ | 实时 |

## 行为定位

内容会根据访客在浏览会话期间的操作和兴趣自动进行调整。 这通常称为&#x200B;_下一页面或同一页面个性化_。

### 主页、冒险和杂志页面

这些体验会根据您当前的浏览行为（实时个性化）立即显示。 Adobe Experience Platform Edge Network用于做出实时个性化决策。

| 页面 | 您将会看到的内容 | 如何测试 | 体验 |
|------|-----------------|-------------|------------|
| [主页](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 个性化的&#x200B;**适合家庭的Adventures英雄横幅**，其中有一个家庭在湖边骑车，推广引导式体验，从而创建共享记忆 | 访问[巴厘岛冲浪营](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"}或[玛莱美食之旅](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"}，然后返回主页 | ![主页 — 家庭冒险英雄](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [冒险](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | WKND的专家合作伙伴提供以骑车为重点的&#x200B;**“免费自行车Tune Up”促销英雄**，其中包含“We&#39;ve Got You Covered”消息和免费自行车维护优惠 | 访问任何与自行车相关的冒险（例如，[骑行Tuscany](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}），然后导航到“冒险”页面 | ![冒险 — 免费自行车Tune Up英雄](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [冒险](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | 一个以露营主题的&#x200B;**装备系列英雄**，展示重要的露营设备（睡袋、夹克、靴子），并附带“您的下一个冒险从正确的装备开始”消息 | 访问任何与露营相关的冒险（例如，[Yosemite Backpacking](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}），然后导航到“冒险”页面 | ![冒险 — 营地装备收藏英雄](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [杂志](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | 时间敏感的&#x200B;**杂志促销活动**，以卷筒的WKND杂志为主题，并配以著名的“SALE！” 徽章和特殊阅读器定价问题，以及户外系列 | 阅读一个或多个杂志文章（例如，[滑雪旅行](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"}），然后导航到杂志登陆页面 | ![杂志 — 销售英雄](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### 冒险和杂志页面（批次）

这些体验基于历史行为，并在下次访问时或当天晚些时候显示（批量个性化）。 数据会被聚合并处理为配置文件属性，并激活到Adobe Experience Platform Edge Network。

| 页面 | 您将会看到的内容 | 如何测试 | 体验 |
|------|-----------------|-------------|------------|
| [冒险](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | 一个以冲浪为主题的主角，在棕榈树下摆放着&#x200B;**彩色冲浪板**，其中包含“您的冲浪历程从这里开始”的信息以及根据您的兴趣策划的冲浪目的地内容 | 访问多个[与冲浪相关的冒险](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"}，然后第二天返回到“冒险”页面 | ![冒险 — 冲浪英雄](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [杂志](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | **个性化杂志订阅优惠**，采用经典的大众面包车，提供世界旅游目的地，强调“您的个性化杂志体验”，具有独家订阅者权益 | 阅读3篇或更多[杂志文章](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"}，然后第二天返回杂志登陆页面 | ![杂志 — 订阅英雄](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**了解更多：**&#x200B;是否准备好在您自己的AEM网站上实施行为定位？ 从[行为定位教程](./use-cases/behavioral-targeting.md)开始，了解完整的设置过程。

## 已知用户个性化

基于跨多个系统的数据（包括购买历史记录和客户生命周期阶段）构建的完整客户配置文件提供个性化体验。 Adobe Experience Platform Edge Network用于做出实时个性化决策。

### 主页主页

WKND主页主页主页主页横幅根据经过身份验证的用户配置文件进行个性化。 使用这些演示帐户进行测试，以查看个性化体验：

| 页面 | 您将会看到的内容 | 如何测试 | 配置文件上下文 | 体验 |
|------|-----------------|-------------|-----------------|------------|
| [主页](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 内部滑雪用品店提供&#x200B;**高级滑雪装备，并提供“额外25%优惠”**&#x200B;促销活动，提供专家打包提示，为即将到来的滑雪之旅做好准备 | 使用`rwilson/rwilson`登录并刷新页面 | 最近购买了滑雪冒险，因此追加销售滑雪装备 | ![家庭 — 滑雪装备追加销售](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**了解更多：**&#x200B;是否准备好在您自己的AEM网站上实施已知用户个性化？ 从[已知用户的Personalization教程](./use-cases/known-user-personalization.md)开始，了解完整的设置过程。

## A/B测试（试验）

测试不同的内容变体以确定哪些变体最符合您的业务目标。 Adobe Target会随机向访客提供各种变体，并跟踪表现更好的访客。 这通常称为&#x200B;_试验_。

### 主页精选文章

WKND主页运行活动A/B测试，该测试包含&#x200B;_Camping in Western Australia_&#x200B;精选文章的三个变体。 每个访客均会被随机分配以查看以下变体之一：

| 页面 | 您将会看到的内容 | 如何测试 | 体验 |
|------|-----------------|-------------|------------|
| [主页](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 在“我们的精选”部分中随机分配的三个精选文章变体之一： **“脱机：横跨西澳大利亚的史诗级露营路线”**&#x200B;或&#x200B;**“漫游：西澳大利亚的露营冒险”**（或第三个变体），每个变体都具有独特的图片和消息传递以测试哪个变体最能引起共鸣 | 使用不同的浏览器访问主页，使用无痕模式/私有模式或清除Cookie以查看不同的变体 | ![A/B测试变体](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**了解详情：**&#x200B;是否准备好在您自己的AEM网站上实施A/B测试？ 从[试验（A/B测试）教程](./use-cases/experimentation.md)开始，了解完整的设置过程。


## 后续步骤

准备好在您自己的AEM网站上实施个性化了吗？ 从[Personalization概述](./overview.md)开始，了解完整的设置过程。


