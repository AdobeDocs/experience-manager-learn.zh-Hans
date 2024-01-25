---
title: 为Asset compute可扩展性设置帐户和服务
description: 开发Asset compute工作人员需要访问帐户和服务，包括Microsoft或Amazon提供的AEMas a Cloud Service、App Builder和云存储。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 228
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# 设置帐户和服务

本教程要求预配以下服务并通过学习者的Adobe ID访问这些服务。

所有Adobe服务都必须可以通过同一Adobe组织使用您的Adobe ID进行访问。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 配置可能需要2 - 10天
+ 云存储
   + [Azure Blob存储](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>在继续学习本教程之前，请确保您能够访问所有上述服务。
> 
> 请查阅以下有关如何设置和提供所需服务的章节。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

需要访问AEMas a Cloud Service环境，才能配置AEM Assets处理配置文件以调用自定义Asset compute工作程序。

理想情况下，沙盒程序或非沙盒开发环境可供使用。

请注意，本地AEM SDK不足以完成本教程，因为本地AEM SDK无法与Asset compute微服务通信，而是需要真正的AEMas a Cloud Service环境。

## App Builder{#app-builder}

此 [App Builder](https://developer.adobe.com/app-builder/) framework用于生成自定义操作并将其部署到Adobe的无服务器平台Adobe I/O Runtime。 AEMAsset compute项目是专门构建的App Builder项目，通过处理配置文件与AEM Assets集成，并提供访问和处理资源二进制文件的功能。

要获取对App Builder的访问权限，请注册预览。

1. [注册App Builder试用版](https://developer.adobe.com/app-builder/trial/).
1. 请等待大约2 - 10天，直到电子邮件通知您已配置好，然后再继续学习本教程。
   + 如果您不确定是否已配置，请继续执行后续步骤，如果您无法创建 __App Builder__ 中的项目 [Adobe Developer控制台](https://developer.adobe.com/console/) 您尚未配置。

## 云存储

本地开发Asset compute项目需要云存储。

在将Asset compute工作程序部署到Adobe I/O Runtime以供AEMas a Cloud Service直接使用时，此云存储不是绝对必需的，因为AEM提供了从中读取资产并将资源写入演绎版的云存储。

### Microsoft Azure Blob存储{#azure-blob-storage}

如果您还没有Microsoft Azure Blob Storage的访问权限，请注册 [12个月免费帐户](https://azure.microsoft.com/en-us/free/).

但是，本教程将使用Azure Blob Storage [Amazon S3](#amazon-s3) 也可以仅用作教程的次要变量。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_配置Azure Blob Storage的点进（无音频）_

1. 登录 [Microsoft Azure帐户](https://azure.microsoft.com/en-us/account/).
1. 导航至 __存储帐户__ Azure服务部分
1. 点按 __+添加__ 创建新的Blob存储帐户
1. 新建 __资源组__ 例如： `aem-as-a-cloud-service`
1. 提供 __存储帐户名称__&#x200B;例如： `aemguideswkndassetcomput`
   + 此 __存储帐户名称__  用于 [配置云存储](../develop/environment-variables.md) 在本地Asset compute开发工具中
   + 此 __访问密钥__ 在以下情况下，也需要与存储帐户关联 [配置云存储](../develop/environment-variables.md).
1. 将其他所有内容保留为默认值，然后点按 __审核+创建__ 按钮
   + （可选）选择 __位置__ 靠近你。
1. 查看配置请求以了解正确性，然后点击 __创建__ 按钮（如果满意）

### Amazon S3{#amazon-s3}

使用 [Microsoft Azure Blob存储](#azure-blob-storage) 但是，建议您完成本教程 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可以使用。

如果使用Amazon S3存储，请在以下情况下指定Amazon S3云存储凭据 [配置项目的环境变量](../develop/environment-variables.md#amazon-s3).

如果您需要专门为此教程配置云存储，我们建议使用 [Azure Blob存储](#azure-blob-storage).
