---
title: 设置BPA和CAM项目
description: 了解Best Practices Analyzer和Cloud Acceleration Manager如何为迁移到AEMas a Cloud Service提供自定义指南。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 3%

---

# Best Practices Analyzer和Cloud Acceleration Manager

了解Best Practices Analyzer(BPA)和Cloud Acceleration Manager(CAM)如何为迁移到AEMas a Cloud Service提供自定义指南。 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## 使用BPA和CAM

![BPA和CAM高级图](assets/bpa-cam-diagram.png)

应将BPA包安装在生产AEM 6.x环境的克隆上。 BPA将生成一个报表，然后该报表可以上传到CAM中，该报表将为需要进行的关键活动提供指导，以便迁移到AEMas a Cloud Service。

## 关键活动

+ 克隆生产6.x环境。 在迁移内容和重构代码时，克隆生产环境对于测试各种工具和更改将非常有价值。
+ 从 [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 并在AEM 6.x克隆环境中安装。
+ 使用BPA工具生成可上传到Cloud Acceleration Manager(CAM)的报表。 CAM通过 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ 使用CAM提供有关需要对当前代码库和环境进行哪些更新以移动到AEMas a Cloud Service的指导。

## 动手练习

通过尝试通过实践练习学到的知识来运用知识。

在尝试动手练习之前，请确保您已观看并了解上述视频，以及以下材料：

+ [对AEMas a Cloud Service的思考](./introduction.md)
+ [什么是AEMas a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [AEM as a Cloud Service 的架构](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [可变和不可变内容](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [AEM as a Cloud Service和AEM 6.x开发方面的差异](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">最佳实践分析器操作</div>
            <p style="margin:1rem 0">
                浏览最佳实践分析器(BPA)，并通过针对包含示例违规的旧版WKND代码库运行该分析器来查看结果。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">试用最佳实践分析器</span>
            </a>
        </td>
    </tr>
</table>


## 其他资源

+ [下载Best Practices Analyzer](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)