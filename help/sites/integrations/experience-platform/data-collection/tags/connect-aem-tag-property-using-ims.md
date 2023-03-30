---
title: 使用IMS将AEM与标记属性连接
description: 了解如何使用AEM中的IMS配置将AEM与标记属性连接。 此设置会使用Launch API验证AEM，并允许AEM通过Launch API进行通信以访问标记属性。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 使用IMS将AEM与标记属性连接{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>在AEM产品UI、内容和文档中正在实施将Adobe Experience Platform Launch重命名为一组数据收集技术的过程，因此此处仍在使用术语Launch。

了解如何使用AEM中的IMS(Identity Management系统)配置将AEM与标记属性连接。 此设置会使用Launch API验证AEM，并允许AEM通过Launch API进行通信以访问标记属性。

## 创建或重用IMS配置

需要使用Adobe Developer控制台项目的IMS配置，才能将AEM与新创建的标记属性相集成。 此配置允许AEM使用Launch API与标记应用程序通信，而IMS则会处理此集成的安全方面。

每当配置AEM as Cloud Service环境时，都会自动创建一些IMS配置，例如Asset compute、Adobe Analytics和Adobe启动。 自动创建 **Adobe启动** 如果您使用的是AEM 6.X环境，则可以使用IMS配置，或者应创建新的IMS配置。

自动创建审阅 **Adobe启动** IMS配置。

1. 在AEM中，打开 **工具** 菜单

1. 在安全部分中，选择Adobe IMS配置。

1. 选择 **Adobe启动** 卡片，单击 **属性**，查看 **证书** 和 **帐户** 选项卡。 然后，单击 **取消** 返回而不修改任何自动创建的详细信息。

1. 选择 **Adobe启动** 卡片，这次点击 **检查运行状况**，您应会看到 **成功** 如下所示的消息。

   ![Adobe启动正常的IMS配置](assets/adobe-launch-healthy-ims-config.png)


## 后续步骤

[在AEM中创建LaunchCloud Service配置](create-aem-launch-cloud-service.md)
