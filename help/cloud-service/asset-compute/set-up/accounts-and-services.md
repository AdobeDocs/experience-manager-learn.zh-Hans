---
title: 为Asset compute可扩展性设置帐户和服务
description: 开发Asset compute工作人员需要访问帐户和服务，包括由Microsoft或Amazon提供的AEMas a Cloud Service、应用程序生成器和云存储。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# 设置帐户和服务

本教程要求以下服务可进行配置，并可通过学习者的Adobe ID访问。

所有Adobe服务都必须使用您的Adobe ID通过同一Adobe组织进行访问。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [应用程序生成器](#app-builder)
   + 配置可能需要2到10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在继续阅读本教程之前，请确保您有权访问上述所有服务。
> 
> 请查看下面有关如何设置和提供所需服务的章节。

## AEMas a Cloud Service{#aem-as-a-cloud-service}

要配置AEM Assets处理配置文件以调用自定义Asset compute工作程序，需要访问AEMas a Cloud Service环境。

理想情况下，可以使用沙盒项目或非沙盒开发环境。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与Asset compute微服务通信，而是需要真正的AEMas a Cloud Service环境。

## 应用程序生成器{#app-builder}

的 [应用程序生成器](https://developer.adobe.com/app-builder/) 框架用于构建自定义操作并将其部署到Adobe的无服务器平台Adobe I/O Runtime。 AEMAsset compute项目是特别构建的App Builder项目，它们通过处理配置文件与AEM Assets集成，并提供访问和处理资产二进制文件的功能。

要获取应用程序生成器的访问权限，请注册以进行预览。

1. [注册以试用App Builder](https://developer.adobe.com/app-builder/trial/).
1. 请等待大约2 - 10天，直到通过电子邮件通知您已进行配置，然后再继续学习教程。
   + 如果您不确定是否已配置，请继续执行后续步骤，如果您无法创建 __应用程序生成器__ 项目 [Adobe Developer控制台](https://developer.adobe.com/console/) 您尚未进行配置。

## 云存储

本地开发Asset compute项目需要云存储。

当Asset compute工作程序部署到Adobe I/O Runtime以供AEMas a Cloud Service直接使用时，并非严格要求使用此云存储，因为AEM提供了从中读取资产并将其写入的云存储。

### Microsoft Azure Blob Storage{#azure-blob-storage}

如果您还没有Microsoft Azure Blob Storage的访问权限，请注册 [免费12个月帐户](https://azure.microsoft.com/en-us/free/).

但是，本教程将使用Azure Blob Storage [Amazon S3](#amazon-s3) 只能与本教程的次要变体一起使用。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_点进配置Azure Blob Storage（无音频）_

1. 登录到 [Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/).
1. 导航到 __存储帐户__ Azure服务部分
1. 点按 __+添加__ 创建新的Blob存储帐户
1. 新建 __资源组__ 根据需要，例如： `aem-as-a-cloud-service`
1. 提供 __存储帐户名称__，例如： `aemguideswkndassetcomput`
   + 的 __存储帐户名称__  用于 [配置云存储](../develop/environment-variables.md) 在本地Asset compute开发工具中
   + 的 __访问密钥__ 与存储帐户关联时 [配置云存储](../develop/environment-variables.md).
1. 将其他所有内容保留为默认值，然后点按 __查看+创建__ 按钮
   + （可选）选择 __位置__ 离你近。
1. 查看配置请求以获取正确性，然后点按 __创建__ 按钮

### Amazon S3{#amazon-s3}

使用 [Microsoft Azure Blob Storage](#azure-blob-storage) 但是，建议您完成本教程 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可使用。

如果使用Amazon S3存储，请在 [配置项目的环境变量](../develop/environment-variables.md#amazon-s3).

如果您需要专门在本教程中配置云存储，我们建议您使用 [Azure Blob存储](#azure-blob-storage).
