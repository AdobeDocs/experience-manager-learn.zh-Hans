---
title: 設定批次資料設定
description: 設定批次資料設定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 建立批次設定

若要使用批次API，請建立批次設定，並根據該設定執行執行。 以下影片示範如何使用API建立批次組態

>[!NOTE]
>請確定AEM使用者屬於 ```forms-users``` 群組進行API呼叫。


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 建立批次設定

以下是建立批次設定的POST端點

```xml
<baseURL>/config
```

以下是在建立批次設定時需要指定的最小設定。 這必須在HTTP要求內文中傳遞為JSON物件

```
{
	"configName": "monthlystatements",
	"dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
	"outputTypes": [
		"PDF"
	],
	"template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## 驗證批次設定

若要驗證批次設定是否成功建立，您可以對以下端點進行GET請求呼叫


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP請求內文中傳遞空白的JSON物件即可
