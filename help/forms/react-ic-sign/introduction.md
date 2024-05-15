---
title: 使用AEM Forms和Acrobat Sign的React应用程序
description: Acrobat Sign和AEM Forms可自动执行复杂的事务，并在无缝数字体验中包含法律电子签名。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# AEM Forms与Acrobat Sign Web窗体


本教程将指导您完成使用从提交的数据生成交互式通信文档的用例。 [React](https://react.dev/) 应用程序和演示生成的文档，以供使用Acrobat Sign Webform进行签名。

以下是用例的流程

* 用户在React应用程序中填写表单。
* 表单数据将提交到AEM Forms端点以生成交互式通信文档。
* 使用生成的文档创建Acrobat Sign构件URL。
* 为用户签名文档提供呼叫应用程序的构件URL。

## 先决条件

用例需要以下各项才能正常工作：

* 带有Forms附加组件包的AEM服务器
* An [Acrobat Sign应用程序的集成密钥](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 后续步骤

写入 [用于生成交互式通信文档的自定义OSGi服务](./create-ic-document.md) 使用文档记录的API
