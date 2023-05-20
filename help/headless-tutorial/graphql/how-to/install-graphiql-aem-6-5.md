---
title: 在AEM 6.5上安裝GraphiQL IDE
description: 瞭解如何在AEM 6.5上安裝和設定GraphiQL IDE
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 17%

---

# 在AEM 6.5上安裝GraphiQL IDE

在AEM 6.5中，必須手動安裝GraphiQL IDE工具。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**。
1. 搜尋「GraphiQL」(請務必包含 **i** 在 **GraphiQL**)。
1. 下載最新版本 **GraphiQL Content Package v.x.x.x**.

   ![下載GraphiQL套件](assets/graphiql/software-distribution.png)

   zip檔案是可直接安裝的AEM套件。

1. 從AEM「開始」功能表，瀏覽至 **工具** > **部署** > **套件**.
1. 单击&#x200B;**上传软件包**，然后选择在之前步骤中下载的软件包。单击&#x200B;**安装**&#x200B;可安装软件包。

   ![安裝GraphiQL套件](assets/graphiql/install-graphiql-package.png)

1. 導覽至 **CRXDE Lite** > **存放庫面板** >選取 `/content/graphiql` 節點(例如， <http://localhost:4502/crx/de/index.jsp#/content/graphiql>)。
1. 在 **屬性** 標籤變更值 `endpoint` 屬性至 `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![端點屬性值變更](assets/graphiql/endpoint-prop-value-change.png)

1. 導覽至 **Web主控台設定** UI >搜尋 **CSRF篩選器** 設定(例如，<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 在 `Excluded Paths` 屬性名稱欄位更新，WKND GraphQL端點路徑 `/content/cq:graphql/wknd-shared/endpoint`.

![排除路徑屬性值變更](assets/graphiql/exclude-paths-value-change.png)

1. 存取GraphiQL編輯器，使用 `//HOST:PORT/content/graphiql.html`，並確認您可以建構新查詢或執行現有查詢。 (例如 <http://localhost:4502/content/graphiql.html>)

![GraphiQL編輯器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>若要支援特定專案的GraphQL結構描述和查詢執行，您必須對進行對應的變更 `endpoint` 和 `Excluded Paths` 上述步驟的值。
