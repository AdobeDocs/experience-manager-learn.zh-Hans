---
title: AEMas a Cloud Service缓存
description: AEMas a Cloud Service缓存的一般概述。
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEMas a Cloud Service缓存

在AEMas a Cloud Service中，了解缓存至关重要。 缓存涉及存储和重用之前获取的数据，以提高系统效率并减少加载时间。 此机制可显着加快内容交付、提升网站性能并优化用户体验。

AEMas a Cloud Service具有多个缓存层，并且Author和Publish服务中的策略有所不同。

![AEMas a Cloud Service缓存概述](./assets/overview/all.png){align="center"}

## AEM缓存

AEMas a Cloud Service具有强大、可配置的多层缓存策略，包括CDN、AEM Dispatcher和可选的客户管理的CDN。 可以微调跨层的缓存以优化性能，从而确保AEM仅提供最佳体验。 AEM对Author和Publish服务的缓存存在不同疑虑。 探索以下每项服务的缓存策略。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish服务" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish服务缓存">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish服务缓存">AEM Publish服务缓存</a></p>
            <p class="is-size-6">AEM Publish服务使用托管的CDN和AEM Dispatcher来优化最终用户Web体验。</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM Author服务缓存" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM Author服务缓存">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM Author服务缓存">AEM Author服务缓存</a></p>
                <p class="is-size-6">AEM Author服务使用托管的CDN来提供优化的创作体验。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学习</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
