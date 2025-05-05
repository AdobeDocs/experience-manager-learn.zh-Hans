---
title: AEM Assets微服务和迁移到AEM as a Cloud Service
description: 了解AEM Assets as a Cloud Service的资源计算微服务如何让您自动高效地为资源生成任何演绎版，从而取代传统AEM Workflow的这一角色。
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# AEM Assets微服务 — 移至AEM as a Cloud Service

了解AEM Assets as a Cloud Service的资源计算微服务如何让您自动高效地为资源生成任何演绎版，从而取代传统AEM Workflow的这一角色。

>[!VIDEO](https://video.tv.adobe.com/v/3454295?quality=12&learn=on&captions=chi_hans)

## 工作流迁移工具

![资产工作流迁移工具](./assets/asset-workflow-migration.png)

作为重构代码库的一部分，请使用[资产工作流迁移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=zh-Hans)迁移现有工作流，以使用AEM as a Cloud Service中的Asset Compute微服务。

## 关键活动

+ 使用[Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator)工具迁移资源处理工作流以使用Asset Compute微服务。
+ 设置[本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)并部署更新的工作流。 对于复杂的工作流，可能需要手动进行调整。
+ 继续使用AEM SDK在本地开发环境中进行迭代，直到更新的工作流与功能对等相匹配。
+ 将更新的代码库部署到AEM as a Cloud Service开发环境，并继续验证。

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [以不同的方式思考AEM as a Cloud Service](./introduction.md)
+ [入门培训](./onboarding.md)

此外，请确保您已完成之前的实践练习：

+ [搜索和索引实践练习](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上传资产的实际操作</div>
            <p style="margin:1rem 0">
                了解如何使用“aem-upload”npm CLI模块定义AEM Assets处理配置文件并将其分配给文件夹，以及将资源上传到AEM。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">尝试资产管理</span>
            </a>
        </td>
    </tr>
</table>
