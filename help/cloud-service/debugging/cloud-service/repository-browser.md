---
title: 使用存放庫瀏覽器偵錯AEM
description: 存放庫瀏覽器是功能強大的工具，可提供AEM基礎資料存放區的可見度，允許輕鬆偵錯AEMas a Cloud Service環境。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 使用存放庫瀏覽器偵錯AEMas a Cloud Service

存放庫瀏覽器是功能強大的工具，可提供AEM基礎資料存放區的可見度，允許輕鬆偵錯AEMas a Cloud Service環境。 存放庫瀏覽器支援生產、暫存和開發以及作者、發佈和預覽服務上AEM的資源和屬性的唯讀檢視。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

存放庫瀏覽器是 __僅限__ 可在AEMas a Cloud Service環境中取得(使用 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 以偵錯本機AEM SDK)。

## 存取存放庫瀏覽器

若要存取AEMas a Cloud Service上的存放庫瀏覽器：

1. 確保您的使用者擁有 [必要的存取權](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 登入 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 選取包含AEMas a Cloud Service環境的程式以進行偵錯
1. 開啟 [開發人員主控台](./developer-console.md) 與要偵錯的AEMas a Cloud Service環境相對應
1. 選取 __存放庫瀏覽器__ 標籤
1. 選取要瀏覽的AEM服務層
   + 所有作者
   + 所有發行者
   + 所有預覽
1. 選取 __開啟存放庫瀏覽器__

「存放庫瀏覽器」會以唯讀模式為選取的服務層（製作、發佈或預覽）開啟，顯示您的使用者有權存取的資源與屬性。

## 發佈和預覽存取權

預設情況下，對發佈或預覽的存取是有限的，減少了存放庫瀏覽器中的可用資源。 [若要檢視發佈（或預覽）上的所有資源，請將使用者新增至發佈（或預覽）管理員角色。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
