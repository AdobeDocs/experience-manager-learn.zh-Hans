---
title: 将有效负载文档写入文件系统
description: 将有效负荷文件夹下的写入文档添加到文件系统的自定义流程步骤
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 将文档写入文件系统

常见的用例是将工作流中生成的文档写入文件系统。
此自定义工作流处理步骤使工作流文档写入文件系统变得容易。
自定义进程采用以下逗号分隔的参数

```java
ChangeBeneficiary.pdf,c:\confirmation
```

第一个参数是要保存到文件系统中的文档的名称。 第二个参数是要保存文档的文件夹位置。 例如，在上述使用案例中，文档被写入 `c:\confirmation\ChangeBeneficiary.pdf`

以下屏幕抓图显示了您需要传递给自定义流程步骤的参数
![write-payload-file-system](assets/write-payload-file-system.png)

[可以从此处下载自定义捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
