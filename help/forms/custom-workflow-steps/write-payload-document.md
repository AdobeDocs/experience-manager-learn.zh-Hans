---
title: 将负载文档写入文件系统
description: 自定义进程步骤，将负载文件夹下的写文档添加到文件系统
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

常见用例是将工作流中生成的文档写入文件系统。
通过此自定义工作流流程步骤，可以轻松地将工作流文档写入文件系统。
自定义过程采用以下以逗号分隔的参数

```java
ChangeBeneficiary.pdf,c:\confirmation
```

第一个参数是要保存到文件系统的文档的名称。 第二个参数是要保存文档的文件夹位置。 例如，在上述用例中，文档将被写入 `c:\confirmation\ChangeBeneficiary.pdf`

以下屏幕快照显示了您需要传递到自定义流程步骤的参数
![write-payload-file-system](assets/write-payload-file-system.png)

[可从此处下载自定义包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
