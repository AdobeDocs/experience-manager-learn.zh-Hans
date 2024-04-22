---
title: 将数据与XDP模板合并
description: 通过将数据与模板合并来创建PDF文档
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# 将数据与XDP模板合并

下一步是将XML数据与模板合并以生成PDF。 然后，将发送此PDF以供使用Adobe Sign进行签名。

## 使用OutputService生成PDF

此 [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) OutputService的方法用于生成PDF。
随后使用Adobe Sign REST API发送生成的PDF以进行签名。

