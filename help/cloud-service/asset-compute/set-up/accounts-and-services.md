---
title: 为Asset compute可扩展性设置帐户和服务
description: 开发Asset compute工作者需要访问帐户和服务，包括作为Cloud Service的AEM、Adobe项目Firefly以及由Microsoft或Amazon提供的云存储。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# 设置帐户和服务

本教程要求以下服务能够供应并通过学员的Adobe ID访问。

所有Adobe服务必须通过同一Adobe组织(使用您的Adobe ID)访问。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe项目FireFly](#adobe-project-firefly)
   + 资源调配可能需要2-10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在继续完成本教程之前，请确保您有权访问上述所有服务。
> 
> 查看以下各节，了解如何设置和提供所需服务。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

要配置AEM Assets处理用户档案以调用自定义Cloud Service环境，需要访问AEM作为Asset compute。

理想情况下，可以使用沙箱项目或非沙箱开发环境。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与Asset computemicroservices通信，而是需要真正的AEM作为Cloud Service环境。

## Adobe项目Firefly{#adobe-project-firefly}

[Adobe项目Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)框架用于构建自定义操作并将其部署到Adobe的无服务器平台Adobe I/O Runtime。 AEMAsset compute项目是专门构建的Firefly项目，它们通过处理用户档案与AEM Assets集成，并提供访问和处理资产二进制文件的能力。

要访问Project Firefly，请注册预览。

1. [注册Project Firefly预览](https://adobeio.typeform.com/to/obqgRm)。
1. 请等待大约2 - 10天，直到通过电子邮件通知您已配置好，然后继续教程。
   + 如果您不确定是否已设置，请继续执行后续步骤，如果无法在[Adobe开发者控制台](https://console.adobe.io)中创建&#x200B;__项目Firefly__&#x200B;项目，则仍未设置。

## 云存储

本地开发存储项目需要云Asset compute。

将Asset compute工作线程部署到Adobe I/O Runtime以供AEM直接用作Cloud Service时，不严格要求使用此云存储，因为提供了从中读取资产和写入资产的云存储。

### Microsoft Azure Blob存储{#azure-blob-storage}

如果您尚无权访问Microsoft Azure Blob存储，请注册一个[免费的12个月帐户](https://azure.microsoft.com/en-us/free/)。

本教程将使用Azure Blob存储，但[AmazonS3](#amazon-s3)只能使用本教程的较小变体。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_点进设置Azure Blob存储（无音频）_


1. 登录您的[Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/)。
1. 导航到&#x200B;__存储帐户__ Azure服务部分
1. 点按&#x200B;__+ Add__&#x200B;以创建新的Blob存储帐户
1. 根据需要新建一个&#x200B;__资源组__，例如：`aem-as-a-cloud-service`
1. 提供&#x200B;__存储帐户名称__，例如：`aemguideswkndassetcomput`
   + __存储帐户名称__&#x200B;将用于[配置本地Asset compute开发工具的云存储](../develop/environment-variables.md)
   + [配置云存储](../develop/environment-variables.md)时，还需要与存储帐户关联的&#x200B;__访问密钥__。
1. 将其他所有内容保留为默认值，然后点按&#x200B;__审阅+创建__&#x200B;按钮
   + 或者，选择您附近的&#x200B;__位置__。
1. 检查供应请求是否正确，如果符合要求，请点按&#x200B;__创建__&#x200B;按钮

### AmazonS3{#amazon-s3}

建议使用[Microsoft Azure Blob存储](#azure-blob-storage)完成本教程，但也可以使用[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)。

如果使用AmazonS3存储，请在[配置项目的环境变量](../develop/environment-variables.md#amazon-s3)时指定AmazonS3云存储凭据。

如果您需要特别为本教程配置云存储，我们建议使用[Azure Blob存储](#azure-blob-storage)。
