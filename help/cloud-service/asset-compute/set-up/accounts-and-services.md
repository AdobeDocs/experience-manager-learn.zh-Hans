---
title: 为Asset compute可扩展性设置帐户和服务
description: 开发Asset compute工作人员需要访问帐户和服务，包括AEM as a Cloud Service、Adobe项目Firefly以及Microsoft或Amazon提供的云存储。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---

# 设置帐户和服务

本教程要求以下服务可进行配置，并可通过学习者的Adobe ID访问。

所有Adobe服务都必须使用您的Adobe ID通过同一Adobe组织进行访问。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe项目FireFly](#adobe-project-firefly)
   + 配置可能需要2到10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在继续阅读本教程之前，请确保您有权访问上述所有服务。
> 
> 请查看下面有关如何设置和提供所需服务的章节。

## AEM as aCloud Service{#aem-as-a-cloud-service}

要配置AEM Assets处理配置文件以调用自定义Asset compute工作程序，需要访问AEM as aCloud Service环境。

理想情况下，可以使用沙盒项目或非沙盒开发环境。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与Asset compute微服务通信，而是需要一个真正的AEM作为Cloud Service环境。

## Adobe项目Firefly{#adobe-project-firefly}

[Adobe项目Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)框架用于构建自定义操作并将其部署到Adobe的无服务器平台Adobe I/O Runtime。 AEMAsset compute项目是特别构建的Firefly项目，它们通过处理配置文件与AEM Assets集成，并提供访问和处理资产二进制文件的功能。

要访问项目Firefly，请注册预览。

1. [注册项目Firefly预览](https://adobeio.typeform.com/to/obqgRm)。
1. 请等待大约2 - 10天，直到通过电子邮件通知您已进行配置，然后再继续学习教程。
   + 如果您不确定是否已配置，请继续执行后续步骤，如果无法在[Adobe开发人员控制台](https://console.adobe.io)中创建&#x200B;__项目Firefly__&#x200B;项目，则您仍未配置。

## 云存储

本地开发Asset compute项目需要云存储。

当Asset compute工作程序部署到Adobe I/O Runtime以供AEM as a Cloud Service直接使用时，并非完全需要此云存储，因为AEM提供了从中读取资产并将其写入的云存储。

### Microsoft Azure Blob Storage{#azure-blob-storage}

如果您还没有Microsoft Azure Blob Storage的访问权限，请注册一个[免费的12个月帐户](https://azure.microsoft.com/en-us/free/)。

本教程将使用Azure Blob Storage，但是[Amazon S3](#amazon-s3)只能使用本教程的次要变体。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_点进配置Azure Blob Storage（无音频）_


1. 登录到您的[Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/)。
1. 导航到&#x200B;__存储帐户__ Azure服务部分
1. 点按&#x200B;__+添加__&#x200B;以创建新的Blob存储帐户
1. 根据需要创建新的&#x200B;__资源组__，例如：`aem-as-a-cloud-service`
1. 提供&#x200B;__存储帐户名称__，例如：`aemguideswkndassetcomput`
   + __存储帐户名称__&#x200B;将用于[配置本地Asset compute开发工具的云存储](../develop/environment-variables.md)
   + 在[配置云存储](../develop/environment-variables.md)时，还需要与存储帐户关联的&#x200B;__访问密钥__。
1. 将其他所有内容保留为默认值，然后点按&#x200B;__查看+创建__&#x200B;按钮
   + 或者，选择您附近的&#x200B;__位置__。
1. 查看配置请求以了解正确性，并点按&#x200B;__创建__&#x200B;按钮（如果已满足）

### Amazon S3{#amazon-s3}

建议使用[Microsoft Azure Blob Storage](#azure-blob-storage)来完成本教程，但也可以使用[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)。

如果使用Amazon S3存储，请在[配置项目的环境变量](../develop/environment-variables.md#amazon-s3)时指定Amazon S3云存储凭据。

如果您需要专门在本教程中配置云存储，我们建议您使用[Azure Blob Storage](#azure-blob-storage)。
