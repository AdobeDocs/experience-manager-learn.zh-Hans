---
title: 远程调试AEM SDK
description: AEM SDK的本地快速启动允许从IDE进行远程Java调试，从而允许您在AEM中逐步执行实时代码，以了解确切的执行流程。
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 远程调试AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM SDK的本地快速启动允许从IDE进行远程Java调试，从而允许您在AEM中逐步执行实时代码，以了解确切的执行流程。

若要将远程调试器连接到AEM，必须使用允许IDE连接到AEM SDK的特定参数(`-agentlib:...`)来启动IDE的本地快速启动。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK仅支持Java 11
+ `address`指定AEM侦听远程调试连接的端口，并可更改为本地开发计算机上的任何可用端口。
+ 最后一个参数(例如 `aem-author-p4502.jar`)是AEM SKD快速入门Jar。 这可以是AEM创作服务(`aem-author-p4502.jar`)或AEM Publish服务(`aem-publish-p4503.jar`)。


## IDE设置说明

大多数Java IDE都支持对Java程序进行远程调试，但每个IDE的具体设置步骤各不相同。 请查看IDE的远程调试设置说明，了解确切步骤。 通常，IDE配置需要：

+ 主机AEM SDK的本地快速启动正在侦听，其名称为`localhost`。
+ 端口AEM SDK的本地快速启动正在侦听远程调试连接，该端口是在启动AEM SDK的本地快速启动时由`address`参数指定的端口。
+ 有时候，必须指定向远程调试提供源代码的Maven项目；这是您的OSGi捆绑包maven项目项目。

### 设置说明

+ [VS代码Java远程调试器设置](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA远程调试器设置](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse远程调试器设置](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
