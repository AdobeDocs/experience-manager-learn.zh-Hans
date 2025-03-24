---
title: 示例项目
description: 可在您的环境中导入和执行的示例项目
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 在本地环境中测试

* 导入项目

   * 下载并提取[示例项目](./assets/formsdocumentservices.zip)
   * 打开首选的&#x200B;**Java开发环境**（IntelliJ IDEA、Eclipse或VS代码）并将项目作为Maven项目导入
* 配置凭据

   * 找到文件`resources/credentials/server_credentials.json`
   * 打开它并&#x200B;**更新特定于您环境的凭据**。
   * 请确保它包括以下内容的有效值：
     `clientId`、`clientSecret`、`adobeIMSV3TokenEndpointURL`和
     `scopes`

* 执行Main类

   * 导航到`src/main/java/Main.java`并运行主方法

* 验证执行
   * 在终端窗口中验证输出
