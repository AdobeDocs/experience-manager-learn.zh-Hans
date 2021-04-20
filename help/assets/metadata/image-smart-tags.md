---
title: 使用AEM Assets为图像添加智能标签
description: 图像的智能标记可根据图像内容自动智能地向图像资产中添加元数据标记，从而增强AEM搜索功能。
topic: Content Management
feature: Smart Tags
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 645
thumbnail: 17019.jpg
translation-type: tm+mt
source-git-commit: a5fb96275194ddc46169832c0fb79a2587833564
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 5%

---


# 图像智能标签

AEM资产的图像智能标记可以自动向图像资产添加派生的元数据标记，从而增强AEM资产的搜索功能，同时通过更轻松、更快地找到正确的图像来改善创作体验。

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## 为AEM 6.x{#set-up}设置

>[!NOTE]
> 图像的智能标记会作为Cloud Service自动为AEM配置。

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

在使用智能内容服务之前，请确保执行以下操作以创建Adobe I/O集成：

* 具有组织管理员权限的Adobe ID帐户
* 您的组织已启用智能内容服务

视频详细说明了配置用于智能标记图像的Adobe I/O智能内容服务所需的以下任务。

* 在AEM中创建智能内容服务配置以生成公钥。 为 OAuth 集成获取公共证书。
* 在Adobe I/O中创建集成，并上传生成的公钥。
* 使用API密钥和Adobe I/O中的其他凭据配置您的AEM实例。
* （可选）在资产上传时启用自动标记。

## 其他资源

* [AEM Assets Smart Tags文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
