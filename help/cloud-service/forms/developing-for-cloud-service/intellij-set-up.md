---
title: 安装IntelliJ社区版
description: 安装AEM项目并将其导入IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 65
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# 安装IntelliJ

安装 [IntelliJ社区版](https://www.jetbrains.com/idea/download/#section=windows). 在安装过程中建议时，您可以接受默认设置。

## 导入AEM项目

* 启动IntelliJ
* 导入您在前一步中创建的AEM项目。 导入项目后，屏幕应如下所示 ![aem-banking-app](assets/aem-banking-app.png). 通常，您将使用核心、ui.apps、ui.config和ui.content子项目。
* 如果未看到Maven和Terminal窗口，请转到“查看” — >“工具”窗口，然后选择“Maven和Terminal”

## 添加字体模块

如果要在PDF文件中使用自定义字体，则需要将自定义字体推送到AEM Forms CS实例。 请按照以下步骤操作

* 创建名为的文件夹 **字体** 在C:\CloudManager\aem-banking-application中
* 提取内容 [font.zip](assets/fonts.zip) 到新创建的字体文件夹中
* 字体模块中包含一些自定义字体。您可以将组织的自定义字体添加到字体模块的C:\CloudManager\aem-banking-application\fonts\src\main\resources文件夹中
* 打开C:\CloudManager\aem-banking-application\pom.xml文件
* 添加以下行  ```<module>fonts</module>``` 在pom.xml的模块部分中
* 保存您的pom.xml
* 刷新IntelliJ中的aem-banking-application项目

带有字体模块的项目结构
![fonts-module](assets/fonts-module.png)

项目POM中包含的字体模块
![fonts-pom](assets/fonts-module-pom.png)

## 后续步骤

[设置Git](./setup-git.md)
