---
title: 配置批量数据配置
description: 配置批量数据配置
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

# 创建批次配置

要使用批处理API，请创建批处理配置并基于该配置执行运行。 以下视频演示了使用API创建批量配置

>[!NOTE]
>请确保AEM用户属于 ```forms-users``` 组进行API调用。


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 创建批次配置

以下是创建批处理配置的POST端点

```xml
<baseURL>/config
```

以下是创建批处理配置时需要指定的最低配置。 需要在HTTP请求正文中将此作为JSON对象传递

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

## 验证批次配置

要验证是否成功创建了批量配置，您可以对以下端点进行GET请求调用


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP请求正文中传递空JSON对象即可
