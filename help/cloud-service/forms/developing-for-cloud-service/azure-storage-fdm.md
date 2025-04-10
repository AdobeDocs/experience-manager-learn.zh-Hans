---
title: 正在将云服务配置和表单数据模型推送到云实例
description: 创建基于Azure存储表单数据模型的自适应表单并将其推送到云实例。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# 在您的项目中包含云服务配置

创建一个名为“FormTutorial”的配置容器来保存您的云服务配置
通过提供Azure存储帐户详细信息和Azure访问密钥，在“FormTutorial”容器中为Azure存储创建一个名为“FormsCSAndAzureBlob”的云服务配置。

在IntelliJ中打开您的AEM项目。 确保添加文件夹FormTutorial，如ui.content项目中所示
![云服务配置](assets/cloud-services-configuration.png)

确保在ui.content项目的filter.xml中添加以下条目

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 在您的项目中包含表单数据模型

根据您在上一步中创建的云服务配置创建表单数据模型。 要在项目中包含表单数据模型，请在intelliJ中的AEM项目中创建相应的文件夹结构。 例如，我的表单数据模型位于名为“注册”的文件夹中
![fdm-content](assets/ui-content-fdm.png)

在ui.content项目的filter.xml中包含相应的条目

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>现在，当您使用Cloud Manager构建和部署项目时，必须在云服务配置中重新输入Azure访问密钥。 为避免重新输入访问键，建议使用[下一篇文章](./context-aware-fdm.md)中所述的环境变量创建上下文感知配置

## 后续步骤

[创建上下文感知配置](./context-aware-fdm.md)
