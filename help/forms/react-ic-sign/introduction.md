---
title: 使用AEM Forms和Acrobat Sign的React應用程式
description: Acrobat Sign和AEM Forms可自動化複雜的交易，並加入法律電子簽章，以提供順暢的數位體驗。
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

# AEM Forms與Acrobat Sign網路表單


本教學課程將逐步引導您瞭解使用提交的資料產生互動式通訊檔案的使用案例。 [React](https://react.dev/) 應用程式並展示產生的檔案，以供使用Acrobat Sign網頁表單簽名。

以下是使用案例的流程

* 使用者在React應用程式中填寫表單。
* 表單資料會提交至AEM Forms端點，以產生互動式通訊檔案。
* 使用產生的檔案建立Acrobat Sign Widget url。
* 向呼叫應用程式呈現Widget URL，以便使用者簽署檔案。

## 前提条件

您需要下列專案使用案例：

* 具有Forms附加元件套件的AEM伺服器
* 一個 [Acrobat Sign應用程式的整合金鑰](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 后续步骤

寫入 [自訂OSGi服務以產生互動式通訊檔案](./create-ic-document.md) 使用已記錄的API
