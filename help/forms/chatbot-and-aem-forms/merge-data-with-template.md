---
title: 将数据与XDP模板合并
description: 通过将数据与模板合并来创建PDF文档
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# 将数据与XDP模板合并

下一步是将XML数据与模板合并，以生成PDF。 随后，将使用Adobe Sign发送此PDF以供签名。

## 使用OutputService生成PDF

OutputService的[generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-)方法用于生成PDF。
随后使用Adobe Sign REST API发送生成的PDF以供签名。
