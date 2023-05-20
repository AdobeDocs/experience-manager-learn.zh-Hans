---
title: 將裝載檔案寫入檔案系統
description: 自訂程式步驟，將位於承載資料夾下的寫入檔案新增至檔案系統
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 將檔案寫入檔案系統

常見的使用案例是將工作流程中產生的檔案寫入檔案系統。
此自訂工作流程程式步驟可讓您輕鬆將工作流程檔案寫入檔案系統。
自訂程式會採用下列逗號分隔引數

```java
ChangeBeneficiary.pdf,c:\confirmation
```

第一個引數是您要儲存至檔案系統的檔名稱。 第二個引數是您要儲存檔案的資料夾位置。 例如在上面的使用案例中，檔案被寫入 `c:\confirmation\ChangeBeneficiary.pdf`

以下熒幕擷圖顯示您需要傳遞至自訂流程步驟的引數
![write-payload-file-system](assets/write-payload-file-system.png)

[自訂套件組合可從這裡下載](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
