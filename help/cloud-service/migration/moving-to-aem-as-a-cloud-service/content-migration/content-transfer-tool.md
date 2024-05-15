---
title: 使用内容传输工具进行内容迁移
description: 了解内容传输工具如何帮助您将内容从AEM 6迁移到as a Cloud ServiceAEM。
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
duration: 1362
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---


# 内容传输工具

了解内容传输工具如何帮助您将内容从AEM 6.3+as a Cloud Service迁移到AEM。

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## 使用内容传输工具

![内容传输工具生命周期](../assets/content-transfer-tool.png)

内容传输工具安装在AEM 6.3+上，并且会将内容传输到AEMas a Cloud Service。

## 关键活动

+ 下载 [最新内容传输工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2).
+ 将AEM Author 6.3+的最终内容传输到AEMas a Cloud Service创作服务。
   + 在包含要传输的最终内容的AEM 6.3+作者上安装内容传输工具。
   + 批量运行内容传输工具，传输内容集。
+ 将AEM Publish 6.3+的最终内容传输到AEMas a Cloud Service发布服务。
   + 在包含要传输的最终内容的AEM 6.3+ Publish上安装内容传输工具。
   + 批量运行内容传输工具，传输内容集。
+ （可选）在AEMas a Cloud Service上使用“增补”内容，方式是自上次内容传输以来传输新内容

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [AEM 现代化工具](../aem-modernization-tools.md)
+ [入门培训](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

此外，请确保您已完成之前的实践练习：

+ [Dispatcher实践练习](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="实践练习GitHub存储库" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用内容传输工具动手</div>
            <p style="margin:1rem 0">
                探索内容传输工具如何自动将内容从AEM 6移至AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">试用内容传输工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他资源

+ [下载内容传输工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [批量导入服务操作方法视频](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

