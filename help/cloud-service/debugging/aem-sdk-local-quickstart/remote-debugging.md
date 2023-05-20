---
title: 遠端偵錯AEM SDK
description: AEM SDK的本機Quickstart允許從IDE進行遠端Java偵錯，讓您逐步完成AEM中的即時程式碼執行，以瞭解確切的執行流程。
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: 45e7c58efd1d89537752fe7f890c0e80f7be7d67
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# 遠端偵錯AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM SDK的本機Quickstart允許從IDE進行遠端Java偵錯，讓您逐步完成AEM中的即時程式碼執行，以瞭解確切的執行流程。

若要將遠端偵錯工具連線至AEM，AEM SDK的本機Quickstart必須使用特定引數啟動(`-agentlib:...`)，允許IDE連線到它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK僅支援Java 11
+ `address` 指定AEM接聽遠端偵錯連線的連線埠，並可變更為本機開發電腦上的任何可用連線埠。
+ 最後一個引數(例如 `aem-author-p4502.jar`)是AEM SKD Quickstart Jar。 這可以是AEM Author服務(`aem-author-p4502.jar`)或AEM Publish服務(`aem-publish-p4503.jar`)。


## IDE設定指示

大多數Java IDE都支援遠端偵錯Java程式，但每個IDE的正確設定步驟有所不同。 請檢閱IDE的遠端偵錯設定指示，以取得確切步驟。 通常IDE配置需要：

+ 主機AEM SDK的本機Quickstart正在接聽，也就是 `localhost`.
+ 連線埠AEM SDK的本機Quickstart正在接聽遠端偵錯連線，該連線是由 `address` 引數(啟動AEM SDK的本機快速入門時)。
+ 有時候，您必須指定提供原始程式碼給遠端偵錯的Maven專案；這是您的OSGi套件maven專案專案。

### 設定指示

+ [VS程式碼Java遠端偵錯工具設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA遠端偵錯工具設定](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse遠端偵錯工具設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
