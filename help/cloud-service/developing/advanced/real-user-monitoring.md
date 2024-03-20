---
title: Real User Monitoring (RUM)
description: 了解AEMas a Cloud Service网站中的Real User Monitoring (RUM)。
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Real User Monitoring (RUM)

了解AEMas a Cloud Service网站中的Real User Monitoring (RUM)。 了解如何启用RUM、收集哪些数据以及如何使用RUM数据优化网站上的用户体验。

## 概述

Real User Monitoring (RUM)是一种用于 _收集、衡量和分析用户交互和体验_ 与网站进行实时交互。 它提供了有关网站访客如何与您的网站进行交互的见解，包括他们的行为、性能和整体体验。 这是通过将一小段JavaScript代码注入网站页面来实现的。

使用JavaScript代码，RUM在用户与您的网站交互时直接从用户的浏览器捕获数据。 此数据可用于识别和诊断性能问题、优化用户体验以及改善业务成果。

AEMas a Cloud Service中的RUM功能可全面查看您网站的用户体验。 它会为用户访问的每个页面(URL)捕获以下关键量度：

- [最大内容绘制(LCP)](https://web.dev/articles/lcp)  — 测量加载性能。
- [累积版面偏移(CLS)](https://web.dev/articles/cls)  — 测量视觉稳定性。
- [下一道绘画交互(INP)](https://web.dev/articles/inp)  — 衡量交互性。
- 页面查看次数 — 测量页面被查看的次数。

它还捕获网站的404错误和页面查看图表。

LCP、CLS和INP指标是 [核心Web虚拟](https://web.dev/articles/vitals) 这是一组与网站速度、响应速度和视觉稳定性相关的量度。 Google使用这些指标衡量用户在网站上的体验，这些指标对于搜索引擎排名非常重要。

## 启用RUM

要为您的AEMCS网站启用RUM，请参阅 [如何设置Real User监控服务](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

AEMCS中RUM的主要详细信息包括：

- RUM仅适用于AEMCS的发布服务，这意味着JavaScript代码仅插入到发布环境中。
- 此 `com.adobe.granite.webvitals.WebVitalsConfig` OSGi配置控制包含和排除路径，这些是存储库路径而非URL路径。
- 默认情况下 `/content` 包括路径。
- 要排除路径，请添加 `AEM_WEBVITALS_EXCLUDE` Cloud Manager环境变量，请参阅 [添加环境变量](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). 路径以逗号分隔。
- OOTB代码负责将JavaScript代码插入页面。

### 验证

要验证您的网站是否启用了RUM，请查看已发布页面的HTML源并搜索以下脚本块：

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM数据收集

- RUM数据的收集方式 `sampleRUM()` 函数。 在上例中，检查点为 `top`， `load`， `click`， `lazy`、和 `cwv`.
- 检查点是在加载页面并与之交互的序列中的命名事件。

另请参阅 [Real User Monitoring Service和Privacy](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) 和 [正在收集哪些数据](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) 以了解更多详细信息。

## RUM数据视图

要查看所需的RUM数据 `domainkey`，Adobe会将其作为RUM设置的一部分提供。 RUM仪表板位于 [https://data.aem.live/](https://data.aem.live/) 并且您可以使用域密钥和url访问它。

例如，下面的屏幕截图显示了AEM WKND网站的RUM仪表板。

![RUM仪表板](./assets/rum/RUM-Dashboard-WKND.png)

RUM仪表板提供以下关键见解：

- **绩效指标** - LCP、CLS、INP和Pageviews。
- **错误量度** - 404错误。
- **页面查看图表**  — 一段时间内的页面查看次数。

## 如何使用RUM数据

利用以上见解，您可以优化网站上的用户体验。 例如：

- 减少LCP、CLS和INP以提高页面加载性能和交互性。 请参阅 [如何改进LCP](https://web.dev/articles/lcp#improve-lcp)， [如何改进CLS](https://web.dev/articles/cls#improve-cls) 和 [如何改进INP](https://web.dev/articles/inp#improve-inp)以了解更多详细信息。
- 要改善用户体验，请修复404错误。
- 要了解用户行为并优化内容，请分析页面查看图表。

Adobe建议定期查看RUM仪表板，尤其是在主要或次要发布后。

另请参阅 [谁能从真正的用户监控服务中受益](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) 以了解更多详细信息。
