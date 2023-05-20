---
title: 測試歡迎套件解決方案
description: 部署解決方案資產以測試解決方案
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 部署和測試範例資產

[安裝歡迎套件套件](assets/welcomekit.zip). 此套件包含頁面範本、列出資產的自訂元件、範例工作流程、電子郵件範本和要包含在歡迎套件中的範例pdf檔案。
[安裝歡迎套件套件](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). 此套件組合包含建立頁面的程式碼，以及傳回要在網頁中顯示的資產的Java類別。
[安裝最適化表單範例](assets/account-openeing-form.zip)
設定Day CQ Mail Service。 工作流程會將歡迎套件連結傳送至需要正確設定SMTP的表單提交者。
根據您的要求設定工作流程的傳送電子郵件元件
[預覽表單](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
輸入您的詳細資料，並選取一或多個共同基金，然後提交表單。您應會收到一封包含歡迎套件連結的電子郵件
