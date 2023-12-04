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
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 233
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 执行批处理配置

要运行批处理，请向以下API发出POST请求

```xml
<baseURL>/confi/<configName>/execution
```

此API需要空json对象作为请求正文中的参数。
此API在由其标识的响应标头中返回唯一URL **位置** 键。
对此唯一URL的GET请求将告知您批次执行的状态

以下视频演示了批量配置的触发

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
