---
title: 为Asset Compute可扩展性设置帐户和服务
description: 开发Asset Compute工作程序需要访问帐户和服务，包括AEM as a Cloud Service、App Builder以及Microsoft或Amazon提供的云存储。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# 设置帐户和服务

本教程要求预配以下服务并通过学习者的Adobe ID访问这些服务。

所有Adobe服务都必须可以通过同一Adobe组织，使用您的Adobe ID进行访问。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 配置可能需要2 - 10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card)

>[!WARNING]
>
>在继续学习本教程之前，请确保您能够访问所有上述服务。
> 
> 请查阅以下有关如何设置和提供所需服务的章节。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

需要访问AEM as a Cloud Service环境，才能配置AEM Assets处理配置文件以调用自定义Asset Compute工作程序。

理想情况下，沙盒程序或非沙盒开发环境可供使用。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与Asset Compute微服务通信，而是需要真正的AEM as a Cloud Service环境。

## App Builder{#app-builder}

[App Builder](https://developer.adobe.com/app-builder/)框架用于生成自定义操作并将其部署到Adobe的无服务器平台Adobe I/O Runtime。 AEM Asset Compute项目是专门构建的App Builder项目，通过处理配置文件与AEM Assets集成，并提供访问和处理资源二进制文件的功能。

要访问App Builder，请注册预览。

1. [注册App Builder试用版](https://developer.adobe.com/app-builder/trial/)。
1. 请等待大约2 - 10天，直到电子邮件通知您已配置好，然后再继续学习本教程。
   + 如果您不确定是否已配置，请继续后续步骤，并且如果您无法在[App Builder](https://developer.adobe.com/console/)中创建&#x200B;__Adobe Developer Console__&#x200B;项目，则您仍未配置。

## 云存储

Asset Compute项目的本地开发需要云存储。

在将Asset Compute工作程序部署到Adobe I/O Runtime以供AEM as a Cloud Service直接使用时，并非完全需要此云存储，因为AEM提供了从中读取资产并将资产写入其中的演绎版的云存储。

### Microsoft Azure Blob存储{#azure-blob-storage}

如果您还没有Microsoft Azure Blob Storage的访问权限，请注册[免费的12个月帐户](https://azure.microsoft.com/en-us/free/)。

本教程将使用Azure Blob Storage，但是[Amazon S3](#amazon-s3)也可以使用，并且只对该教程稍作改动。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_配置Azure Blob Storage的点进（无音频）_

1. 登录到您的[Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/)。
1. 导航到&#x200B;__存储帐户__ Azure服务部分
1. 点按&#x200B;__+添加__&#x200B;以创建新的Blob存储帐户
1. 根据需要创建新的&#x200B;__资源组__，例如： `aem-as-a-cloud-service`
1. 提供&#x200B;__存储帐户名称__，例如： `aemguideswkndassetcomput`
   + 用于在本地Asset Compute开发工具中[配置云存储](../develop/environment-variables.md)的&#x200B;__存储帐户名称__
   + 在[配置云存储](../develop/environment-variables.md)时，还需要与存储帐户关联的&#x200B;__访问密钥__。
1. 将其他所有内容保留为默认值，并点按&#x200B;__审阅+创建__&#x200B;按钮
   + （可选）选择靠近您的&#x200B;__位置__。
1. 检查配置请求是否正确，如果正确，请点按&#x200B;__创建__&#x200B;按钮

### Amazon S3{#amazon-s3}

建议使用[Microsoft Azure Blob Storage](#azure-blob-storage)完成本教程，但也可以使用[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card)。

如果使用Amazon S3存储，请在[配置项目的环境变量](../develop/environment-variables.md#amazon-s3)时指定Amazon S3云存储凭据。

如果需要专门为此教程配置云存储，我们建议使用[Azure Blob存储](#azure-blob-storage)。
