---
title: 在AEMas a Cloud Service中搜索和索引
description: 了解AEMas a Cloud Service的搜索索引、如何转换AEM 6索引定义以及如何部署索引。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# 搜索和索引

了解AEMas a Cloud Service的搜索索引、如何将AEM 6索引定义转换为与AEMas a Cloud Service兼容，以及如何将索引部署到AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## 索引转换工具

![索引转换工具](./assets/index-converter.png)

在重构代码库时，请使用 [索引转换工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 将自定义Oak索引定义转换为AEMas a Cloud Service兼容的索引定义。

## 关键活动

+ 使用 [Adobe I/O工作流迁移器](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 用于迁移资产处理工作流以使用Asset compute微服务的工具。
+ 设置 [本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans) 并部署自定义索引。 确保更新的索引是最新的。
+ 将更新的代码库部署到AEMas a Cloud Service开发环境，并继续验证。
+ 如果修改现成的索引 **始终** 从最新版本上运行的AEMas a Cloud Service环境中复制最新索引定义。 根据需要修改复制的索引定义。

## 动手练习

通过尝试通过实践练习学到的知识来运用知识。

在尝试动手练习之前，请确保您已观看并了解上述视频，以及以下材料：

+ [对AEMas a Cloud Service的思考](./introduction.md)
+ [存储库现代化](./repository-modernization.md)

此外，请确保您已完成之前的动手练习：

+ [内容传输工具动手练习](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用索引的实际操作</div>
            <p style="margin:1rem 0">
                探索定义Oak索引并将其部署到AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">尝试索引</span>
            </a>
        </td>
    </tr>
</table>
