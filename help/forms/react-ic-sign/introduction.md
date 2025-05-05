---
title: 使用AEM Forms和Acrobat Sign的React应用程序
description: Acrobat Sign和AEM Forms可自动执行复杂的事务，并在无缝数字体验中包含法律电子签名。
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# AEM Forms与Acrobat Sign Web窗体


本教程将指导您完成以下用例：使用从[React](https://react.dev/)应用程序提交的数据生成交互式通信文档，并使用Acrobat Sign Web窗体展示生成的签名文档。

以下是用例的流程

* 用户在React应用程序中填写表单。
* 表单数据将提交到AEM Forms端点以生成交互式通信文档。
* 使用生成的文档创建Acrobat Sign构件URL。
* 为用户签名文档提供呼叫应用程序的构件URL。

## 先决条件

用例需要以下各项才能正常工作：

* 带有Forms附加组件包的AEM服务器
* Acrobat Sign应用程序的[集成密钥](https://helpx.adobe.com/cn/sign/kb/how-to-create-an-integration-key.html)

## 后续步骤

编写[自定义OSGi服务以使用文档化的API生成交互式通信文档](./create-ic-document.md)
