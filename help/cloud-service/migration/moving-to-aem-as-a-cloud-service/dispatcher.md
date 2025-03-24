---
title: 在迁移到AEM as a Cloud Service时配置Dispatcher
description: 了解对AEM Dispatcher for AEM as a Cloud Service的重要更改、Dispatcher转换工具以及如何使用Dispatcher Tools SDK。
version: Experience Manager as a Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1618
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

了解适用于AEM as a Cloud Service的AEM Dispatcher，并重点关注适用于AEM 6的Dispatcher的重要更改、Dispatcher转换工具以及如何使用Dispatcher工具SDK。

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher转换器

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

作为重构代码库的一部分，请使用[AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html)将现有的内部部署或Adobe Managed Services Dispatcher配置重构为与AEM as a Cloud Service兼容的Dispatcher配置。

## 关键活动

+ 使用[Adobe I/O Dispatcher Converter工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter)迁移现有的Dispatcher配置。
+ 最佳做法是引用[Dispatcher项目原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud)中的AEM模块。
+ [设置本地Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html)以验证Dispatcher，然后在Cloud Service环境中进行测试。

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [AEM 现代化工具](./aem-modernization-tools.md)
+ [入门培训](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

此外，请确保您已完成之前的实践练习：

+ [Cloud Manager实践练习](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher Tools的动手实践</div>
            <p style="margin:1rem 0">
                探索使用AEM SDK的Dispatcher工具验证Dispatcher配置，以及使用Docker在本地运行AEM Dispatcher。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">试用Dispatcher Tools</span>
            </a>
        </td>
    </tr>
</table>
