---
title: 设置BPA和CAM项目
description: 了解Best Practice Analyzer和Cloud Acceleration Manager如何为迁移到AEMas a Cloud Service提供自定义指南。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Best Practice Analyzer和Cloud Acceleration Manager

了解Best Practice Analyzer(BPA)和Cloud Acceleration Manager(CAM)如何为迁移到AEMas a Cloud Service提供自定义指南。 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## 评估准备情况

![BPA和CAM高级图](assets/bpa-cam-diagram.png)

应将BPA包安装在生产AEM 6.x环境的克隆上。 BPA将生成一个报表，然后该报表可以上传到CAM中，该报表将为需要进行的关键活动提供指导，以便迁移到AEMas a Cloud Service。

### 关键活动

* 克隆生产6.x环境。 在迁移内容和重构代码时，克隆生产环境对于测试各种工具和更改将非常有价值。
* 从 [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 并在AEM 6.x克隆环境中安装。
* 使用BPA工具生成可上传到Cloud Acceleration Manager(CAM)的报表。 CAM通过 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* 使用CAM提供有关需要对当前代码库和环境进行哪些更新以移动到AEMas a Cloud Service的指导。
