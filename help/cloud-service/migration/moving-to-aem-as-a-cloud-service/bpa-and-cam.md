---
title: 设置BPA和CAM项目
description: 了解Best Practices Analyzer和Cloud Acceleration Manager如何提供有关迁移到AEMas a Cloud Service的自定义指南。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 680
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Best Practices Analyzer和Cloud Acceleration Manager

了解Best Practices Analyzer (BPA)和Cloud Acceleration Manager (CAM)如何提供有关迁移到AEMas a Cloud Service的自定义指南。 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## 使用BPA和CAM

![BPA和CAM高级图](assets/bpa-cam-diagram.png)

BPA软件包应安装在AEM 6.x生产环境的克隆上。 BPA将生成一份报告，之后可将其上载到CAM中，该报告将指导您完成迁移到AEMas a Cloud Service所需的关键操作。

## 关键活动

+ 克隆生产6.x环境。 在迁移内容和重构代码时，克隆生产环境对于测试各种工具和更改很有用。
+ 从下载最新的BPA工具 [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 并在您的AEM 6.x克隆环境中安装。
+ 使用BPA工具生成可上传到Cloud Acceleration Manager (CAM)的报告。 通过访问CAM [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ 使用CAM提供有关当前代码库和环境需要哪些更新才能移至AEMas a Cloud Service的指导。

## 实践练习

通过尝试通过这个实践练习学到的知识来应用您的知识。

在尝试动手练习之前，请确保您已观看并了解上述视频以及以下材料：

+ [以不同的方式思考AEMas a Cloud Service](./introduction.md)
+ [什么是AEMas a Cloud Service？](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [AEM as a Cloud Service 的架构](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [可变和不可变内容](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [针对AEMas a Cloud Service和AEM 6.x进行开发的差异](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="实践练习GitHub存储库" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用Best Practices Analyzer实践</div>
            <p style="margin:1rem 0">
                探索最佳实践分析器(BPA)，并通过针对包含示例违规的旧版WKND代码库运行该分析器来查看结果。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">尝试使用最佳实践分析器</span>
            </a>
        </td>
    </tr>
</table>


## 其他资源

+ [下载最佳实践分析器](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)