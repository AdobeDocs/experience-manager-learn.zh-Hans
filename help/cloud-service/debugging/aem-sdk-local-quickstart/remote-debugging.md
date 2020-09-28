---
title: 远程调试AEM SDK
description: AEM SDK的本地快速启动允许从IDE中进行远程Java调试，允许您在AEM中逐步执行实时代码以了解确切的执行流程。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# 远程调试AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDK的本地快速启动允许从IDE中进行远程Java调试，允许您在AEM中逐步执行实时代码以了解确切的执行流程。

要将远程调试器连接到AEM，必须使用特定参数()启动AEM SDK的本地快速`-agentlib:...`启动，以便IDE连接到它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` 指定AEM监听远程调试连接的端口，并可更改为本地开发计算机上的任何可用端口。
+ 最后一个参数(例如 `aem-author-p4502.jar`)是AEM SKD快速启动Jar。 这可以是AEM作者服务(`aem-author-p4502.jar`)或AEM发布服务(`aem-publish-p4503.jar`)。

## IDE设置说明

大多数Java IDE都支持Java项目的远程调试，但每个IDE的确切设置步骤各不相同。 请查看IDE的远程调试设置说明以了解具体步骤。 通常，IDE配置需要：

+ 主机AEM SDK的本地快速启动正在监听，即 `localhost`。
+ 端口AEM SDK的本地快速启动正在侦听远程调试连接，该端口是启动AEM SDK的本 `address` 地快速启动时由参数指定的端口。
+ 有时，必须指定为远程调试提供源代码的Maven项目；这是您的OSGi bundle maven项目。

### 设置说明

+ [VS代码Java远程调试器设置](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote调试器设置](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse Remote调试器设置](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
