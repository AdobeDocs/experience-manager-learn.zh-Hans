---
title: AEM Forms
seo-title: 将自适应表单数据与Acroform合并
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---


# 从Acroform创建模式

下一步是从在前一步中创建的Acroform创建模式。 本教程中提供了一个用于创建模式的范例应用程序。 要创建模式，请按照以下说明操作：

1. 登录 [CRXDE Lite](http://localhost:4502/crx/de)
2. 打开到文件 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 将其更 `saveLocation` 改为硬盘上的相应文件夹。 确保已创建要保存到的文件夹。
4. 将浏览器指向托 [管在AEM](http://localhost:4502/content/DocumentServices/CreateXsd.html) 上的“创建XSD”页面。
5. 拖放Acroform。
6. 检查在步骤3中指定的文件夹。 模式文件将保存到此位置。

## 上传Acroform

要使此演示在您的系统上运行，您需要创建一个名为AEM Assets的 `acroforms` 文件夹。 将Acroform上传到此文 `acroforms` 件夹。

>[!NOTE]
>
>示例代码在此文件夹中查找acroform。 合并自适应表单的提交数据时需要acroform。
