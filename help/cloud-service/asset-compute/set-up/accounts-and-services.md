---
title: 为资产计算可扩展性设置帐户和服务
description: 开发资产计算工作者需要访问帐户和服务，包括AEM作为Cloud Service、Adobe项目Firefly以及Microsoft或Amazon提供的云存储。
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

+ [AEM 云服务](#aem-as-a-cloud-service)
+ [Adobe项目FireFly](#adobe-project-firefly)
   + 资源调配可能需要2-10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在继续完成本教程之前，请确保您有权访问上述所有服务。
> 
> 查看以下各节，了解如何设置和提供所需服务。

## AEM 云服务{#aem-as-a-cloud-service}

要配置AEM Assets处理用户档案以调用自定义资产计算工作器，需要以Cloud Service环境身份访问AEM。

理想情况下，可以使用沙箱项目或非沙箱开发环境。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与资产计算微服务通信，而是需要真的AEM作为Cloud Service环境。

## Adobe项目Firefly{#adobe-project-firefly}

Adobe [项目Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) framework用于构建自定义操作并将其部署到Adobe I/O Runtime，是Adobe的无服务器平台。 AEM资产计算项目是专门构建的Firefly项目，它们通过处理用户档案与AEM Assets集成，并提供访问和处理资产二进制文件的能力。

要访问Project Firefly，请注册预览。

1. [注册Project Firefly预览](https://adobeio.typeform.com/to/obqgRm)。
1. 请等待大约2 - 10天，直到通过电子邮件通知您已配置好，然后继续教程。
   + 如果您不确定是否已设置，请继续执行后续步骤，如果您无法在 __Adobe开发__ 人员控 [制台中创建](https://console.adobe.io) Project Firefly项目，您仍未设置。

## 云存储

本地开发资产计算项目需要云存储。

当资产计算工作线程部署到Adobe I/O Runtime供AEM直接用作Cloud Service时，此云存储并非严格要求，因为AEM提供从中读取资产并写入其演绎版的云存储。

### Microsoft Azure Blob存储{#azure-blob-storage}

如果您尚无权访问Microsoft Azure Blob存储，请注册一个免费的12 [个月帐户](https://azure.microsoft.com/en-us/free/)。

本教程将使用Azure Blob存储, [但是](#amazon-s3) ,AmazonS3也只能使用本教程的较小变体。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_点进设置Azure Blob存储（无音频）_


1. 登录您的 [Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/)。
1. 导航到 __存储帐户__ Azure服务部分
1. 点按 __+添加__ ，以创建新的Blob存储帐户
1. 根据需要 __创建新__ “资源”组，例如： `aem-as-a-cloud-service`
1. 提供 __存储帐户__，例如： `aemguideswkndassetcomput`
   + 存储 __帐户名__ ，将用于为本 [地资产计算开发工](../develop/environment-variables.md) 具配置云存储。
   + 配置 __云存储__ 时，还需要与存储帐户关 [联的访问密钥](../develop/environment-variables.md)。
1. 将所有其他内容保留为默认值，然后点 __按“审阅+创建__ ”按钮
   + （可选）选择 __您__ 附近的位置。
1. 查看供应请求是否正确，如果符合要求，请点 __击__ “创建”按钮

### AmazonS3{#amazon-s3}

建议 [使用Microsoft Azure Blob存储](#azure-blob-storage) ，以完成本教程，但 [也可以使用](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) AmazonS3。

如果使用AmazonS3存储，请在配置项目的存储变量时指 [定AmazonS3云环境凭据](../develop/environment-variables.md#amazon-s3)。

如果您需要特别为本教程配置云存储，我们建议使用 [Azure Blob存储](#azure-blob-storage)。
