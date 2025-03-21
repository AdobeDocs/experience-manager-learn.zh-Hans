---
title: 执行批次配置
description: 通过执行批处理启动文档生成过程
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# 执行批处理配置

要运行批处理，请向以下API发出POST请求

```xml
<baseURL>/confi/<configName>/execution
```

此API需要空json对象作为请求正文中的参数。
此API在响应标头中返回由**位置**密钥标识的唯一URL。
对此唯一URL的GET请求将告知您批次执行的状态

以下视频演示了批量配置的触发

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
