---
title: 执行批次配置
description: 通过执行批处理启动文档生成过程
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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
此API在响应标头中返回由&#x200B;**位置**&#x200B;密钥标识的唯一URL。
对于此唯一URL的GET请求将告知您批量执行的状态

以下视频演示了批量配置的触发

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
