---
title: 配置批量数据配置
description: 配置批量数据配置
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 创建批次配置

要使用批处理API，请创建批处理配置并根据该配置执行运行。 以下视频演示了如何使用API创建批量配置

>[!NOTE]
>请确保AEM用户属于```forms-users```组以进行API调用。


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 创建批次配置

以下是创建批处理配置的POST端点

```xml
<baseURL>/config
```

以下是创建批处理配置时需要指定的最低配置。 这需要在HTTP请求正文中作为JSON对象传递

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

要验证是否成功创建了批次配置，您可以对以下端点进行GET请求调用


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP请求正文中传递一个空JSON对象即可
