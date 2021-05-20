---
title: 带有AEM Forms的Acroforms
seo-title: 将自适应表单数据与Acroform合并
description: Acroforms与AEM Forms集成的第2部分。 从Acroform创建架构。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---


# 从Acroform创建架构

下一步是从前面步骤中创建的Acroform创建架构。 在本教程中，提供了一个用于创建模式的示例应用程序。 要创建架构，请按照以下说明操作：

1. 登录到[CRXDE Lite](http://localhost:4502/crx/de)
2. 打开到文件`/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 将`saveLocation`更改为硬盘上的相应文件夹。 确保已创建要保存到的文件夹。
4. 将您的浏览器指向托管在AEM上的[创建XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html)页面。
5. 拖放Acroform。
6. 选中步骤3中指定的文件夹。 架构文件将保存到此位置。

## 上传Acroform

要使此演示在您的系统上工作，您需要在AEM Assets中创建一个名为`acroforms`的文件夹。 将Acroform上传到此`acroforms`文件夹中。

>[!NOTE]
>
>示例代码会查找此文件夹中的Acroform。 合并自适应表单的提交数据时需要Acroform。
