---
title: 移动到AEMas a Cloud Service时配置Dispatcher
description: 了解对AEM Dispatcher for AEMas a Cloud Service、Dispatcher转换工具以及如何使用Dispatcher Tools SDK的显着更改。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 6%

---


# Dispatcher

了解AEM Dispatcher for AEMas a Cloud Service，重点介绍对Dispatcher for AEM 6、Dispatcher转换工具以及如何使用Dispatcher Tools SDK做出的显着更改。

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

在重构代码库时，请使用 [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 可将现有的内部部署或Adobe Managed Services Dispatcher配置重构为AEMas a Cloud Service兼容的Dispatcher配置。

## 关键活动

+ 使用 [Adobe I/ODispatcher Converter工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 迁移现有Dispatcher配置。
+ 从 [AEM项目原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作为最佳实践。
+ [设置本地Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 在Cloud Service环境中进行测试之前，验证调度程序。

## 动手练习

通过尝试通过实践练习学到的知识来运用知识。

在尝试动手练习之前，请确保您已观看并了解上述视频，以及以下材料：

+ [AEM 现代化工具](./aem-modernization-tools.md)
+ [入门培训](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

此外，请确保您已完成之前的动手练习：

+ [Cloud Manager动手练习](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher工具的实践</div>
            <p style="margin:1rem 0">
                探索如何使用AEM SDK的Dispatcher工具验证Dispatcher配置，以及如何使用Docker在本地运行AEM Dispatcher。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">试用Dispatcher工具</span>
            </a>
        </td>
    </tr>
</table>
