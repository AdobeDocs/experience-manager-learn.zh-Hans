---
title: 使用內容轉移工具進行內容移轉
description: 瞭解內容轉移工具如何協助您將內容從AEM 6移轉至AEMas a Cloud Service。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 2%

---


# 内容传输工具

瞭解內容轉移工具如何協助您將內容從AEM 6.3+移轉至AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## 使用內容轉移工具

![內容轉移工具生命週期](../assets/content-transfer-tool.png)

「內容轉移工具」已安裝在AEM 6.3+上，可將內容轉移到AEMas a Cloud Service。

## 重要活動

+ 下載 [最新的內容轉移工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2).
+ 將AEM Author 6.3+最終內容轉移到AEMas a Cloud ServiceAuthor服務。
   + 在AEM 6.3+作者上安裝「內容轉移工具」，其中包含要轉移的最終內容。
   + 以批次方式執行「內容轉移工具」，轉移內容集。
+ 將AEM Publish 6.3+最終內容轉移到AEMas a Cloud Service發佈服務。
   + 在包含要轉移之最終內容的AEM 6.3+發佈上安裝內容轉移工具。
   + 以批次方式執行「內容轉移工具」，轉移內容集。
+ 可選擇在AEMas a Cloud Service上使用「追加」內容，方式為自上次內容轉移以來轉移新內容

## 實作練習

透過這個實作練習來嘗試您學到的知識，以運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [AEM 现代化工具](../aem-modernization-tools.md)
+ [入门培训](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

此外，請確定您已完成先前的實作練習：

+ [Dispatcher實作練習](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="實作練習GitHub存放庫" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">透過內容轉移工具動手操作</div>
            <p style="margin:1rem 0">
                探索「內容轉移工具」如何自動將內容從AEM 6移至AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">試用內容轉移工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他資源

+ [下載內容轉移工具](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [大量匯入服務操作影片](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

