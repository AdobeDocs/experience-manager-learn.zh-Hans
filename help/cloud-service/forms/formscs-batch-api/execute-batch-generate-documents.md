---
title: 執行批次設定
description: 透過執行批次來啟動檔案產生流程
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 執行批次設定

若要執行批次，請向以下API發出POST請求

```xml
<baseURL>/confi/<configName>/execution
```

此API預期空白json物件為要求內文中的引數。
此API會在由所識別的回應標頭中傳回唯一的URL **位置** 金鑰。
對此唯一URL的GET請求將告知您批次執行的狀態

以下影片示範批次設定的觸發

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
