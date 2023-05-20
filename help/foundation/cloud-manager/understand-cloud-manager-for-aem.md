---
title: 瞭解Adobe Cloud Manager
description: Adobe Cloud Manager提供簡單但強大的解決方案，可輕鬆管理、內部審查和自助服務AEM環境。
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# 瞭解Adobe Cloud Manager

Adobe Cloud Manager提供簡單但強大的解決方案，可輕鬆管理、內部審查和自助服務AEM環境。

## Cloud Manager總覽

本影片系列會探索AEM適用的Cloud Manager的主要功能，包括：

* [项目](#programs)
* [环境](#environments)
* [报表](#reports)
* [CI/CD 生产管道](#cicd-production-pipeline)
* [CI/CD非生產用管道](#cicd-non-production-pipeline)
* [活动](#activity)

如需完整概述，請檢閱 [Cloud Manager使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## 项目 {#programs}

[Cloud Manager計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) 代表支援邏輯業務計畫集合的AEM環境集合，通常會對應到已購買的服務等級協定(SLA)。 例如，一個計畫可能代表AEM資源以支援全球公共網站，而另一個計畫代表內部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) 由AEM Author、AEM Publish和Dispatcher執行個體組成。 不同的環境支援角色，並可使用不同的CI/CD管道參與（如下所述）。 Cloud Manager環境通常有一個生產環境和一個中繼環境。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## 报表 {#reports}

[Cloud Manager報表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) 透過報告及追蹤每個AEM執行個體的各種量度的一組圖表，提供計畫環境和AEM執行個體的檢視。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 生产管道 {#cicd-production-pipeline}

*[在Adobe Cloud Manager中使用CI/CD管道](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 影片系列提供生產管道執行的深入探討，包括探索失敗和成功的部署。*

>[!NOTE]
>
> 透過這些影片，組建、測試和部署時間都已加快，以縮短影片播放時間。 根據專案大小、AEM執行個體數量和UAT流程，完整的管道執行通常需要45分鐘或更長時間（包括強制性的30分鐘效能測試）。

### 配置

此 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 設定會定義起始管道的觸發條件，以及控制生產部署和效能測試引數的引數。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### 管道执行

此 [CI/CD生產管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) 用於透過Stage建置計畫碼並將計畫碼部署到生產環境，減少實現價值的時間。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD非生產用管道 {#cicd-non-production-pipeline}

[CI/CD 非生产管道分为两个类别：代码质量管道和部署管道。](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html)代码质量管道从 Git 分支获取所有代码以生成并对照 Cloud Manager 的代码质量扫描接受评估。部署管道支持将代码从 Git 存储库自动部署到任意非生产环境，这意味着任何已配置的 AEM 环境不是暂存环境或生产环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager提供計畫活動的整合式檢視，列出所有CI/CD管道執行，包括生產和非生產，允許檢視過去和現在的活動，並可以檢視任何活動的詳細資訊。

Cloud Manager也可在每個使用者層級與整合 [Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)，全方位檢視感興趣的事件和動作。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
