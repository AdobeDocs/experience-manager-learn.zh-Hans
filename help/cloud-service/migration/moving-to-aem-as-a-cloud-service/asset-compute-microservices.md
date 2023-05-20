---
title: AEM Assets微服務和移至AEMas a Cloud Service
description: 瞭解AEM Assetsas a Cloud Service的asset compute微服務如何讓您有效率地自動產生資產的任何轉譯，取代傳統AEM Workflow的角色。
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 5%

---

# AEM Assets微服務 — 移至AEMas a Cloud Service

瞭解AEM Assetsas a Cloud Service的asset compute微服務如何讓您有效率地自動產生資產的任何轉譯，取代傳統AEM Workflow的角色。

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## 工作流程移轉工具

![资源工作流迁移工具](./assets/asset-workflow-migration.png)

在重構程式碼庫時，請使用 [資產工作流程移轉工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) 移轉現有工作流程以使用AEMas a Cloud Service中的Asset compute微服務。

## 重要活動

+ 使用 [Adobe I/O工作流程移轉程式](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) 移轉資產處理工作流程以使用Asset compute微服務的工具。
+ 設定 [本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans) 並部署更新的工作流程。 複雜的工作流程可能需要手動調整。
+ 繼續使用AEM SDK在本機開發環境中反複執行，直到更新的工作流程符合功能對等為止。
+ 將更新的程式碼庫部署到AEMas a Cloud Service開發環境，並繼續驗證。

## 實作練習

透過這個實作練習來嘗試您學到的知識，以運用您的知識。

在嘗試實作練習之前，請確定您已觀看並瞭解上述影片和下列資料：

+ [以不同方式思考AEMas a Cloud Service](./introduction.md)
+ [入门培训](./onboarding.md)

此外，請確定您已完成先前的實作練習：

+ [搜尋和建立索引實作練習](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="實作練習GitHub存放庫" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">上傳資產的實際操作</div>
            <p style="margin:1rem 0">
                探索如何使用「aem-upload」npm CLI模組來定義及指派AEM Assets處理設定檔至資料夾，以及上傳資產至AEM。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">嘗試資產管理</span>
            </a>
        </td>
    </tr>
</table>
