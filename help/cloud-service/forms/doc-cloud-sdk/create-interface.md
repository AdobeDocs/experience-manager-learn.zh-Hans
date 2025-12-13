---
title: 创建服务接口
description: 在要公开的接口中定义方法
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
thumbnail: 7825.jpg
jira: KT-7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
duration: 7
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 4%

---


# 接口

创建具有以下2个方法定义的接口。

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {    
    public Document getPDF(String location,String accessToken,String fileName);
    
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
