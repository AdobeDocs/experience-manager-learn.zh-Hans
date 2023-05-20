---
title: 將體驗片段匯出至Adobe Target
description: 瞭解如何將AEM體驗片段發佈和匯出為Adobe Target選件。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---

# 將體驗片段匯出至Adobe Target {#experience-fragment-target}

瞭解如何將AEM體驗片段匯出為Adobe Target選件。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用體驗片段選件建立Target活動](./create-target-activity.md)

## 疑难解答

### 將體驗片段匯出至目標失敗

#### 错误

在沒有Adobe Admin Console中的正確許可權的情況下將體驗片段匯出到Adobe Target，會在AEM Author服務上導致以下錯誤：

    ![Target API UI錯誤](assets/error-target-offer.png)

...和以下日誌訊息 `aemerror` 記錄：

    ![Target API主控台錯誤](assets/target-console-error.png)

#### 解决方法

1. 登入 [Admin Console](https://adminconsole.adobe.com/) 已使用Adobe Target產品設定檔的管理許可權，但AEM整合
2. 選取 __產品> Adobe Target >產品描述檔__
3. 下 __整合__ 索引標籤中，為您的AEMas a Cloud Service環境選取整合(與Adobe I/O專案相同的名稱)
4. 指派 __編輯者__ 或 __核准者__ 角色

   ![Target API錯誤](assets/target-permissions.png)

新增正確許可權至您的Adobe Target整合應可解決此錯誤。

## 支援連結

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
