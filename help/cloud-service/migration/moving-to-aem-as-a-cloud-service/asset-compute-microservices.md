---
title: AEM Assets微服务和迁移到AEMas a Cloud Service
description: 了解AEM Assets as a Cloud Service的asset compute微服务如何允许您自动高效地为资产生成任何演绎版，从而取代传统AEM工作流的这一角色。
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 3%

---

# AEM Assets微服务 — 迁移到AEMas a Cloud Service

了解AEM Assets as a Cloud Service的asset compute微服务如何允许您自动高效地为资产生成任何演绎版，从而取代传统AEM工作流的这一角色。

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## 工作流迁移工具

![资产工作流迁移工具](./assets/asset-workflow-migration.png)

在重构代码库时，请使用 [资产工作流迁移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) 迁移现有工作流以使用AEMas a Cloud Service中的Asset compute微服务。

## 关键活动

+ 使用 [Adobe I/O工作流迁移器](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) 用于迁移资产处理工作流以使用Asset compute微服务的工具。
+ 设置 [本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 和部署更新的工作流。 复杂的工作流可能需要手动调整。
+ 继续使用AEM SDK在本地开发环境中进行迭代，直到更新的工作流与功能对等性匹配为止。
+ 将更新的代码库部署到AEMas a Cloud Service开发环境，并继续验证。

## 动手练习

通过尝试通过实践练习学到的知识来运用知识。

在尝试动手练习之前，请确保您已观看并了解上述视频，以及以下材料：

+ [对AEMas a Cloud Service的思考](./introduction.md)
+ [入门](./onboarding.md)

此外，请确保您已完成之前的动手练习：

+ [搜索并索引实际操作练习](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上传资产的实践</div>
            <p style="margin:1rem 0">
                了解如何使用“aem-upload”npm CLI模块定义AEM Assets处理配置文件并将其分配到文件夹，以及将资产上传到AEM。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">尝试资产管理</span>
            </a>
        </td>
    </tr>
</table>
