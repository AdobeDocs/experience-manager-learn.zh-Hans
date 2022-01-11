---
title: 安装IntelliJ社区版
description: 将AEM项目安装并导入IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# 安装IntelliJ

安装 [IntelliJ社区版](https://www.jetbrains.com/idea/download/#section=windows). 您可以接受默认设置，但建议在安装过程中使用。

## 导入AEM项目

* 启动IntelliJ
* 导入在前面步骤中创建的AEM项目。 导入项目后，屏幕应如下所示 ![aem-banking-app](assets/aem-banking-app.png). 您通常将使用核心项目、ui.apps、ui.config和ui.content子项目。
* 如果看不到Maven和Terminal窗口，请转到“查看” — >“工具窗口”，然后选择“Maven和Terminal”

## 添加字体模块

如果要在PDF文件中使用自定义字体，则需要将自定义字体推送到AEM Forms CS实例。 请按照以下步骤操作

* 创建名为 **字体** C:\CloudManager\aem-banking-application
* 提取的内容 [font.zip](assets/fonts.zip) 到新创建的字体文件夹中
* 字体模块中包含一些自定义字体。您可以将贵组织的自定义字体添加到C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* 打开C:\CloudManager\aem-banking-application\pom.xml文件
* 添加以下行  ```<module>fonts</module>``` 在pom.xml的“模块”部分中
* 保存pom.xml
* 在IntelliJ中刷新aem-banking-application项目

具有字体模块的项目结构
![字体模块](assets/fonts-module.png)

项目POM中包含的字体模块
![fonts-pom](assets/fonts-module-pom.png)
