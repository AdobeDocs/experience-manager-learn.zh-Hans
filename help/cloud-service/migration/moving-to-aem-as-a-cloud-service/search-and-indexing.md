---
title: AEM as a Cloud Service中的搜索和索引编制
description: 了解AEM as a Cloud Service的搜索索引、如何转换AEM 6索引定义以及如何部署索引。
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# 搜索和编制索引

了解AEM as a Cloud Service的搜索索引、如何将AEM 6索引定义转换为与AEM as a Cloud Service兼容的定义，以及如何将索引部署到AEM as a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/3454724?quality=12&learn=on&captions=chi_hans)

## 索引转换器工具

![索引转换器工具](./assets/index-converter.png)

作为重构代码库的一部分，请使用[索引转换器工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter)将自定义Oak索引定义转换为与AEM as a Cloud Service兼容的索引定义。

请查看[索引转换器文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=zh-Hans)，以了解完整的和当前的一组索引转换器功能。

## 关键活动

+ 使用[Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter)工具迁移资源处理工作流以使用Asset Compute微服务。
+ 设置[本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)并部署自定义索引。 确保更新的索引是最新的。
+ 将更新的代码库部署到AEM as a Cloud Service开发环境，并继续验证。
+ 如果修改开箱即用的索引&#x200B;**ALWAYS**，请从运行于最新版本的AEM as a Cloud Service环境中复制最新的索引定义。 修改复制的索引定义以满足您的需要。

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [以不同的方式思考AEM as a Cloud Service](./introduction.md)
+ [存储库现代化](./repository-modernization.md)

此外，请确保您已完成之前的实践练习：

+ [内容传输工具实践练习](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">索引动手</div>
            <p style="margin:1rem 0">
                探索如何定义Oak索引并将其部署到AEM as a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">尝试编制索引</span>
            </a>
        </td>
    </tr>
</table>
