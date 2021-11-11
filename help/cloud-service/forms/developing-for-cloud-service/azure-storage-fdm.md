---
title: 将云服务配置和表单数据模型推送到云实例
description: 基于Azure存储表单数据模型创建自适应表单并将其推送到云实例。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# 将云服务配置包含到您的项目中

创建名为“FormsTutorial”的配置容器以保存云服务配置在“FormsTutorial”容器中为Azure存储创建名为“Azure中的存储表单提交”的云服务配置。 提供Azure存储帐户详细信息和帐户密钥

在IntelliJ中打开您的AEM项目。 确保在ui.content项目中添加如下所示的文件夹FormTutorial
![cloud-services-configuration](assets/cloud-services-configuration.png)

确保在ui.content项目的filter.xml中添加以下条目

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 在项目中包含表单数据模型

根据您在前面步骤中创建的云服务配置创建表单数据模型。 要在项目中包含表单数据模型，请在AEM项目中以IntelliJ创建相应的文件夹结构。 例如，我的表单数据模型位于名为“注册”的文件夹中
![fdm-content](assets/ui-content-fdm.png)

在ui.content项目的filter.xml中包含相应的条目

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>现在，当您构建和部署项目时，该项目将根据云实例中可用的云服务配置创建表单数据模型
