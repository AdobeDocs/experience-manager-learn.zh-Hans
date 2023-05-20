---
title: 使用AEM現代化工具移至AEMas a Cloud Service
description: 瞭解如何使用AEM現代化工具將現有AEM專案和內容升級成相容於AEMas a Cloud Service。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 6%

---


# AEM 现代化工具

瞭解如何使用AEM現代化工具將現有AEM Sites內容升級成與AEMas a Cloud Service相容並符合最佳實務。

## 多合一轉換器

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## 頁面轉換

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## 元件轉換

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## 原則匯入

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## 使用AEM現代化工具

![AEM現代化工具生命週期](./assets/aem-modernization-tools.png)

AEM現代化工具會自動轉換由舊版靜態範本、基礎元件和parsys組成的現有AEM頁面，以使用可編輯範本、AEM核心WCM元件和版面配置容器等現代化方法。

## 重要活動

+ 複製AEM 6.x生產環境，以執行AEM現代化工具
+ 下載並安裝 [最新AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest) 透過「封裝管理程式」在AEM 6.x生產複製上執行

+ [頁面結構轉換器](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) 使用版面配置容器將靜態範本中的現有頁面內容更新為對應的可編輯範本
   + 使用OSGi設定定義轉換規則
   + 對現有頁面執行頁面結構轉換器

+ [元件轉換器](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) 使用版面配置容器將靜態範本中的現有頁面內容更新為對應的可編輯範本
   + 透過JCR節點定義/XML定義轉換規則
   + 對現有頁面執行「元件轉換工具」

+ [原則匯入工具](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) 從設計組態建立原則
   + 使用JCR節點定義/XML定義轉換規則
   + 針對現有設計定義執行原則匯入工具
   + 將匯入的原則套用至AEM元件和容器

## 實作練習

透過這個實作練習來嘗試您學到的知識，以運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [以不同方式思考AEMas a Cloud Service](./introduction.md)
+ [存放庫現代化](./repository-modernization.md)
+ [可變和不可變的內容](../../developing/basics/mutable-immutable.md)
+ [AEM專案結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

此外，請確定您已完成先前的實作練習：

+ [BPA和CAM實作練習](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">AEM現代化動手操作</div>
            <p style="margin:1rem 0">
                探索使用AEM現代化工具更新舊版WKND網站以符合AEMas a Cloud Service最佳實務。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">請試用AEM現代化工具</span>
            </a>
        </td>
    </tr>
</table>

## 其他資源

+ [下載AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM現代化工具檔案](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - AEM現代化套裝簡介](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. 在本機AEM SDK上部署最新化的wknd舊版網站。 AEM ASK可從這裡下載：
   + [软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
