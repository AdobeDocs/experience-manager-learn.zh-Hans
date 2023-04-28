---
title: 使用AEM Forms和Acrobat Sign反应应用程序
description: Acrobat Sign和AEM Forms可自动执行复杂的交易，并将合法的电子签名作为无缝数字体验的一部分。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms与Acrobat Sign Web窗体


本教程将指导您完成使用从 [React](https://react.dev/) 应用程序和演示生成的文档以使用Acrobat Sign Webform进行签名。

以下是用例的流程

* 用户在React应用程序中填写表单。
* 表单数据被提交到AEM Forms端点以生成交互式通信文档。
* 使用生成的文档创建Acrobat Sign小组件URL。
* 向调用应用程序显示小组件URL，以供用户对文档进行签名。

## 前提条件

要使用案例正常工作，您需要满足以下条件：

* 包含Forms的AEM服务器
* 安 [Acrobat Sign应用程序的集成密钥](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 后续步骤

编写 [自定义OSGi服务以生成交互式通信文档](./create-ic-document.md) 使用记录的API
