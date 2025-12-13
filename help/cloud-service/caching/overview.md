---
title: AEM as a Cloud Service 缓存
description: AEM as a Cloud Service 缓存的总体概述
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# AEM as a Cloud Service 缓存

在 AEM as a Cloud Service 中，理解缓存机制至关重要。缓存是指存储并重用先前获取的数据，以提高系统效率并减少加载时间。此机制能够显著加速内容投放，提升网站性能，并优化用户体验。

AEM as a Cloud Service 具有多个缓存层，且在 Author 服务与 Publish 服务之间采用的缓存策略各不相同。

![AEM as a Cloud Service 缓存概述](./assets/overview/all.png){align="center"}

## AEM 缓存

AEM as a Cloud Service 具有强大且可配置的多层缓存策略，包括 CDN、AEM Dispatcher，以及可选的客户自管理 CDN。可以对跨层缓存进行微调以优化性能，确保 AEM 始终提供最佳体验。对于 Author 服务和 Publish 服务，AEM 在缓存方面有不同的关注点。请查看下方各服务的缓存策略。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish 服务" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish 服务缓存">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish 服务缓存">AEM Publish 服务缓存</a></p>
            <p class="is-size-6">AEM Publish 服务使用托管 CDN 和 AEM Dispatcher 来优化终端用户的 Web 体验。</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM Author 服务缓存" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM Author 服务缓存">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM Author 服务缓存">AEM Author 服务缓存</a></p>
                <p class="is-size-6">AEM Author 服务使用托管 CDN 来提供优化的创作体验。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">了解</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
