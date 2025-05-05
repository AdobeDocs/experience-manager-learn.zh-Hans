---
title: 使用AEM现代化工具迁移到AEM as a Cloud Service
description: 了解如何使用AEM现代化工具将现有AEM项目和内容升级为可与AEM as a Cloud Service兼容的项目和内容。
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# AEM 现代化工具

了解如何使用AEM现代化工具将现有AEM Sites内容升级为与AEM as a Cloud Service兼容并符合最佳实践。

## 多功能一体转换器

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## 页面转换

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## 组件转换

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## 策略导入

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## 使用AEM现代化工具

![AEM现代化工具生命周期](./assets/aem-modernization-tools.png)

AEM现代化工具会自动转换由旧版静态模板、基础组件和Parsys组成的现有AEM页面，以使用可编辑模板、AEM核心WCM组件和布局容器等现代方法。

## 关键活动

+ 克隆AEM 6.x生产环境以运行AEM现代化工具
+ 通过包管理器在AEM 6.x生产克隆上下载并安装[最新的AEM现代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest)

+ [页面结构转换器](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html)使用布局容器将现有页面内容从静态模板更新为映射的可编辑模板
   + 使用OSGi配置定义转化规则
   + 针对现有页面运行页面结构转换器

+ [组件转换器](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html)使用布局容器将现有页面内容从静态模板更新为映射的可编辑模板
   + 通过JCR节点定义/XML定义转换规则
   + 针对现有页面运行组件转换器工具

+ [策略导入程序](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html)从设计配置创建策略
   + 使用JCR节点定义/XML定义转换规则
   + 针对现有设计定义运行策略导入程序
   + 将导入的策略应用到AEM组件和容器

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [以不同的方式思考AEM as a Cloud Service](./introduction.md)
+ [存储库现代化](./repository-modernization.md)
+ [可变和不可变内容](../../developing/basics/mutable-immutable.md)
+ [AEM项目结构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=zh-Hans)

此外，请确保您已完成之前的实践练习：

+ [BPA和CAM实践练习](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">AEM现代化实践</div>
            <p style="margin:1rem 0">
                探索使用AEM现代化工具更新旧版WKND网站以符合AEM as a Cloud Service最佳实践。
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

+ [下载AEM现代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM现代化工具文档](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems — 介绍AEM现代化套件](https://helpx.adobe.com/cn/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. 在本地AEM SDK上部署新现代化的wknd旧版网站。 AEM ASK可从此处下载：
   + [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html)。
