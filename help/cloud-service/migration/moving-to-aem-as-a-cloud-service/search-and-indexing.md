---
title: 在AEMas a Cloud Service中搜尋和建立索引
description: 瞭解AEMas a Cloud Service的搜尋索引、如何轉換AEM 6索引定義以及如何部署索引。
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

# 搜尋和建立索引

瞭解AEMas a Cloud Service的搜尋索引、如何將AEM 6索引定義轉換成相容於AEMas a Cloud Service，以及如何將索引部署到AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## 索引轉換工具

![索引轉換工具](./assets/index-converter.png)

在重構程式碼庫時，請使用 [索引轉換工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 將自訂Oak索引定義轉換為與AEMas a Cloud Service相容的索引定義。

## 重要活動

+ 使用 [Adobe I/O工作流程移轉程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 移轉資產處理工作流程以使用Asset compute微服務的工具。
+ 設定 [本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans) 並部署自訂索引。 確保更新的索引是最新的。
+ 將更新的程式碼庫部署到AEMas a Cloud Service開發環境，並繼續驗證。
+ 如果修改開箱即用索引 **一律** 從最新版本上執行之AEMas a Cloud Service環境複製最新索引定義。 修改複製的索引定義以符合您的需求。

## 實作練習

透過這個實作練習來嘗試您學到的知識，以運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [以不同方式思考AEMas a Cloud Service](./introduction.md)
+ [存放庫現代化](./repository-modernization.md)

此外，請確定您已完成先前的實作練習：

+ [內容轉移工具實作練習](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">使用索引的實作</div>
            <p style="margin:1rem 0">
                探索定義及部署Oak索引至AEMas a Cloud Service。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試建立索引</span>
            </a>
        </td>
    </tr>
</table>
