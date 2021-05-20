---
title: 远程调试AEM SDK
description: AEM SDK的本地快速启动允许从IDE进行远程Java调试，从而允许您逐步执行AEM中的实时代码，以了解确切的执行流程。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: 开发
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# 远程调试AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDK的本地快速启动允许从IDE进行远程Java调试，从而允许您逐步执行AEM中的实时代码，以了解确切的执行流程。

要将远程调试器连接到AEM，必须使用特定参数(`-agentlib:...`)启动AEM SDK的本地快速启动，以允许IDE连接到该调试器。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` 指定AEM侦听远程调试连接的端口，并可更改为本地开发计算机上的任何可用端口。
+ 最后一个参数(例如 `aem-author-p4502.jar`)是AEM SKD快速入门Jar。这可以是AEM创作服务(`aem-author-p4502.jar`)或AEM发布服务(`aem-publish-p4503.jar`)。

## IDE设置说明

大多数Java IDE都支持对Java程序进行远程调试，但每个IDE的确切设置步骤各不相同。 请查看IDE的远程调试设置说明，以了解具体步骤。 通常，IDE配置需要：

+ 主机AEM SDK的本地快速启动正在侦听，它为`localhost`。
+ 端口AEM SDK的本地快速启动正在侦听远程调试连接，该连接是启动AEM SDK的本地快速启动时由`address`参数指定的端口。
+ 有时，必须指定为远程调试提供源代码的Maven项目；这是您的OSGi包maven项目。

### 设置说明

+ [与代码Java远程调试器设置](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA远程调试器设置](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [设置Eclipse远程调试器](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
