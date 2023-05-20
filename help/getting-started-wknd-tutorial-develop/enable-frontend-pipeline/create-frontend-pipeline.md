---
title: 使用前端管道部署
description: 瞭解如何建立和執行前端管道，以建立前端資源並部署到AEMas a Cloud Service的內建CDN。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# 使用前端管道部署

在本章中，我們會在Adobe Cloud Manager中建立和執行前端管道。 它只會建置以下位置的檔案： `ui.frontend` 模組並將其部署至AEMas a Cloud Service的內建CDN。 因此請遠離  `/etc.clientlibs` 前端資源傳遞。


## 目標 {#objectives}

* 建立和執行前端管道。
* 確認前端資源不是從提供 `/etc.clientlibs` 但是從開頭為的新主機名稱 `https://static-`

## 使用前端管道

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [更新標準AEM專案](./update-project.md) 已完成。

確定您擁有 [在Cloud Manager中建立和部署管道的許可權](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) 和 [存取AEMas a Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 重新命名現有管道

重新命名現有管道，從 __部署至開發__ 至  __FullStack WKND部署至開發__ 前往 __設定__ 標籤的 __非生產管道名稱__ 欄位。 這是為了明確表示管道是完整棧疊還是前端，只需檢視其名稱。

![重新命名管道](assets/fullstack-wknd-deploy-dev-pipeline.png)


同樣位於 __原始碼__ 索引標籤中，確定存放庫和Git分支欄位值正確，且分支包含您前端管道合約的變更。

![原始程式碼設定管道](assets/fullstack-wknd-source-code-config.png)


## 建立前端管道

至 __僅限__ 從建置和部署前端資源 `ui.frontend` 模組，請執行下列步驟：

1. 在Cloud Manager UI中，從 __管道__ 區段，按一下 __新增__ 按鈕，然後選取 __新增非生產管道__ (或 __新增生產管道__)根據您想要部署至的AEMas a Cloud Service環境而定。

1. 在 __新增非生產管道__ 對話方塊，作為 __設定__ 步驟，選取 __部署管道__ 選項，將其命名為 __前端WKND部署至開發環境__，然後按一下 __繼續__

![建立前端管道設定](assets/create-frontend-pipeline-configs.png)

1. 作為 __原始碼__ 步驟，選取 __前端計畫碼__ 選項，然後從中選擇環境 __符合資格的部署環境__. 在 __原始碼__ 區段會確儲存放庫和Git分支欄位值正確，並且分支具有您前端管道合約的變更。
和 __最重要的是__ 的 __程式碼位置__ 值為的欄位 `/ui.frontend` 最後，按一下 __儲存__.

![建立前端管道原始碼](assets/create-frontend-pipeline-source-code.png)


## 部署順序

* 首次執行新重新命名的 __FullStack WKND部署至開發__ 用於從AEM存放庫中移除WKND clientlib檔案的管道。 最重要的是透過新增以下內容為前端管道合約準備AEM __Sling設定__ 檔案(`SiteConfig`， `HtmlPageItemsConfig`)。

![未設定樣式的WKND網站](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>之後， __FullStack WKND部署至開發__ 管道完成，您將擁有 __未設定樣式__ WKND網站，看起來可能已損毀。 請針對中斷進行規劃，或在奇數小時進行部署，這是您必須在初始切換期間針對的一次性中斷，從使用單一全棧疊管道切換至前端管道。


* 最後，執行 __前端WKND部署至開發環境__ 僅供建置的管道 `ui.frontend` 模組，並將前端資源直接部署至CDN。

>[!IMPORTANT]
>
>您會注意到 __未設定樣式__ WKND網站已恢復正常，這次可以 __前端__ 管道執行比完整棧疊管道快得多。

## 驗證樣式變更和新的傳遞正規化

* 開啟WKND網站的任何頁面，您就能看見文字顏色 __Adobe紅__ 和前端資源(CSS、JS)檔案是從CDN傳遞。 資源要求主機名稱開頭為 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 以及您在中參考的site.js或任何其他靜態資源 `HtmlPageItemsConfig` 檔案。


![新樣式的WKND網站](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>此 `$HASH_VALUE$` 以下與您在中看到的相同 __前端WKND部署至開發環境__  管道的 __內容雜湊__ 欄位。 AEM會收到前端資源的CDN URL的通知，該值會儲存在 `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` 在 __prefixpath__ 屬性。


![雜湊值關聯](assets/hash-value-correlartion.png)



## 恭喜！ {#congratulations}

恭喜，您已建立、執行及驗證只建置和部署WKND Sites專案的「ui.frontend」模組的前端管道。 現在，您的前端團隊可以在完整AEM專案生命週期之外，快速重複網站的設計和前端行為。

## 后续步骤 {#next-steps}

在下一章中， [考量事項](considerations.md)，您將瞭解對前端和後端開發流程的影響。
