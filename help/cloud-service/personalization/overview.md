---
title: 个性化概述
description: 了解如何使用Adobe Target和AEM as a Cloud Service应用程序个性化Adobe Experience Platform网站。
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
exl-id: c4fb11b9-b613-4522-b9da-18d7ae0826ec
source-git-commit: 5b91e7409ff0735bab40d78ad98410ac2ab006ed
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 11%

---

# 个性化概述

了解AEM as a Cloud Service (AEMCS)如何与Adobe Target和Adobe Experience Platform (AEP)集成。 了解如何使用A/B测试提供个性化体验、根据用户的行为定位用户，或使用客户配置文件个性化内容。

## 先决条件

为了演示各种个性化方案，本教程使用示例[AEM WKND](https://github.com/adobe/aem-guides-wknd/)项目。 要跟进，您需要：

- 有权访问的Adobe组织：
   - **AEM as a Cloud Service环境** — 创建和管理内容
   - **Adobe Target** — 用于撰写和交付个性化体验
   - **Adobe Experience Platform应用程序** — 管理客户个人资料和受众
   - AEP中的&#x200B;**标记（以前为Launch）** — 部署Web SDK和自定义JavaScript以进行数据收集和个性化

- 对AEM组件和体验片段的基本了解

- [AEM WKND](https://github.com/adobe/aem-guides-wknd/)项目已部署到您的AEM as a Cloud Service环境。

## 开始使用

在探索特定用例之前，您首先要配置AEM as a Cloud Service进行个性化。 首先，集成Adobe Target和标记，以使用AEP Web SDK实现客户端个性化。 这些基本步骤允许您的AEM页面支持实验、受众定位和实时个性化。

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="集成 Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="集成 Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="集成 Adobe Target">集成Adobe Target</a>
                    </p>
                    <p class="is-size-6">将AEMCS与Adobe Target集成以激活个性化内容作为Adobe Target优惠。</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">集成Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="集成标记" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="集成标记"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="集成标记">集成标记</a>
                    </p>
                    <p class="is-size-6">将AEMCS与标记集成，以插入Web SDK和自定义JavaScript以进行数据收集和个性化。</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">集成标记</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## 用例

探索AEMCS、Adobe Target和Adobe Experience Platform支持的以下常见个性化用例。

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
    {title = Experimentation (A/B Testing)}
    {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
    {image = ./assets/use-cases/experiment/experimentation.png}
    {cta = Learn Experimentation}

* ./use-cases/behavioral-targeting.md
    {title = Behavioral Targeting}
    {description = Learn how to personalize content based on user behavior using Adobe Experience Platform and Adobe Target.}
    {image = ./assets/use-cases/behavioral-targeting/behavioral-targeting.png}
    {cta = Learn Behavioral Targeting}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="试验（A/B 测试）" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="试验（A/B 测试）"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="试验（A/B 测试）">试验（A/B测试）</a>
                    </p>
                    <p class="is-size-6">了解如何使用Adobe Target在AEMCS中测试各种内容变体进行A/B测试。</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习试验</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Behavioral Targeting">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/behavioral-targeting.md" title="行为定位" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/behavioral-targeting/behavioral-targeting.png" alt="行为定位"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" title="行为定位">行为定位</a>
                    </p>
                    <p class="is-size-6">了解如何使用Adobe Experience Platform和Adobe Target根据用户行为使内容个性化。</p>
                </div>
                <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习行为定位</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
