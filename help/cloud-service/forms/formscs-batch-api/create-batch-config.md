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
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 创建批配置

要使用批处理API，请创建批处理配置并根据该配置执行运行。 以下视频演示了如何使用API创建批量配置

>[!NOTE]
>请确保AEM用户属于 ```forms-users``` 进行API调用的组。


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

## 创建批配置

以下是用于创建批量配置的POST端点

```xml
<baseURL>/config
```

以下是创建批处理配置时需要指定的最小配置。 这需要作为JSON对象在HTTP请求正文中传递

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

## 验证批配置

要验证是否成功创建了批量配置，您可以对以下端点进行GET请求调用


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP请求正文中传递一个空的JSON对象

