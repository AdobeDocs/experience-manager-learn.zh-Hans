---
title: 包含AEM Forms的Acroforms
description: 将Acroforms与AEM Forms集成的第2部分。 从Acroform创建架构。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# 从Acroform创建架构

下一步是从上一步创建的Acroform创建架构。 在本教程中提供了一个用于创建架构的示例应用程序。 要创建架构，请按照以下说明操作：

1. 登录 [CRXDE Lite](http://localhost:4502/crx/de)
2. 打开文件 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 更改 `saveLocation` 到硬盘上的相应文件夹。 确保已创建要保存到的文件夹。
4. 将浏览器指向 [创建XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 在AEM上托管的页面。
5. 拖放Acroform。
6. 检查步骤3中指定的文件夹。 架构文件将保存到此位置。

## 上传Acroform

要使此演示在您的系统上正常工作，您需要创建一个名为的文件夹 `acroforms` 在AEM Assets中。 将Acroform上传到此 `acroforms` 文件夹。

>[!NOTE]
>
>示例代码将在此文件夹中查找该缩写。 合并自适应表单提交的数据需要使用acroform。
