---
title: 使用AEM现代化工具迁移到AEMas a Cloud Service
description: 了解如何使用AEM现代化工具升级现有AEM项目和内容，使其as a Cloud Service兼容。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 5%

---


# AEM 现代化工具

了解如何使用AEM现代化工具升级现有的AEM Sites内容，使其as a Cloud Service兼容并符合最佳实践。

## 一体式转换器

>[!VIDEO](https://video.tv.adobe.com/v/338802/?quality=12&learn=on)

## 页面转换

>[!VIDEO](https://video.tv.adobe.com/v/338799/?quality=12&learn=on)

## 组件转换

>[!VIDEO](https://video.tv.adobe.com/v/338788/?quality=12&learn=on)

## 策略导入

>[!VIDEO](https://video.tv.adobe.com/v/338797/?quality=12&learn=on)

## 使用AEM现代化工具

![AEM现代化工具生命周期](./assets/aem-modernization-tools.png)

AEM现代化工具会自动转换由旧版静态模板、基础组件和parsys组成的现有AEM页面，以使用可编辑模板、AEM核心WCM组件和布局容器等现代方法。

## 关键活动

+ 克隆AEM 6.x生产以针对
+ 下载并安装 [最新AEM现代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest) 通过包管理器在AEM 6.x生产克隆上

+ [页面结构转换器](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) 使用布局容器将静态模板中的现有页面内容更新为已映射的可编辑模板
   + 使用OSGi配置定义转化规则
   + 针对现有页面运行页面结构转换器

+ [组件转换器](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) 使用布局容器将静态模板中的现有页面内容更新为已映射的可编辑模板
   + 通过JCR节点定义/XML定义转化规则
   + 针对现有页面运行组件转换器工具

+ [策略导入器](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) 从设计配置创建策略
   + 使用JCR节点定义/XML定义转化规则
   + 根据现有设计定义运行策略导入器
   + 将导入的策略应用于AEM组件和容器

## 动手练习

通过尝试通过实践练习学到的知识来运用知识。

在尝试动手练习之前，请确保您已观看并了解上述视频，以及以下材料：

+ [对AEMas a Cloud Service的思考](./introduction.md)
+ [存储库现代化](./repository-modernization.md)
+ [可变和不可变内容](../../developing/basics/mutable-immutable.md)
+ [AEM项目结构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

此外，请确保您已完成之前的动手练习：

+ [BPA和CAM实操](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">实践AEM现代化</div>
            <p style="margin:1rem 0">
                探索使用AEM现代化工具更新旧版WKND站点，以符合AEMas a Cloud Service最佳实践。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">试用AEM现代化工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他资源

+ [下载AEM Modernizations工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM现代化工具文档](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems — 介绍AEM现代化套件](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. 在本地AEM SDK上部署新近现代化的旧版网站。 AEM ASK可从以下位置下载：
   + [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
