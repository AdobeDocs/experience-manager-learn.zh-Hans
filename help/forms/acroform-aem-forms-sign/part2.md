---
title: 带有AEM Forms的Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: Acroforms与AEM Forms集成的第2部分。 从Acroform创建架构。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# 从Acroform创建架构

下一步是从前面步骤中创建的Acroform创建架构。 在本教程中，提供了一个用于创建模式的示例应用程序。 要创建架构，请按照以下说明操作：

1. 登录 [CRXDE Lite](http://localhost:4502/crx/de)
2. 打开到文件 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 更改 `saveLocation` 到硬盘上的相应文件夹。 确保已创建要保存到的文件夹。
4. 将您的浏览器指向 [创建XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 页面托管在AEM上。
5. 拖放Acroform。
6. 选中步骤3中指定的文件夹。 架构文件将保存到此位置。

## 上传Acroform

要使此演示在您的系统上工作，您需要创建一个名为 `acroforms` 在AEM Assets。 将Acroform上传到此 `acroforms` 文件夹。

>[!NOTE]
>
>示例代码会查找此文件夹中的Acroform。 合并自适应表单的提交数据时需要Acroform。
