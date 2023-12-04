---
title: 使用IMS将AEM Sites与标记属性连接
description: 了解如何使用AEM中的IMS配置将AEM Sites与标记属性连接。 此设置使用Launch API验证AEM，并允许AEM通过Launch API进行通信以访问标记属性。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 81
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# 使用IMS将AEM Sites与标记属性连接{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>AEM产品UI、内容和文档正在实施将Adobe Experience Platform Launch重命名为一组数据收集技术的过程，因此，此处仍使用Launch一词。

了解如何使用AEM中的IMS (Identity Management System)配置将AEM与标记属性连接。 此设置使用Launch API验证AEM，并允许AEM通过Launch API进行通信以访问标记属性。

## 创建或重用IMS配置

需要使用Adobe Developer Console项目的IMS配置才能将AEM与新创建的标记属性集成。 此配置允许AEM使用Launch API与标记应用程序进行通信，并且IMS处理此集成的安全方面。

每当设置AEM as aCloud Service环境时，都会自动创建一些IMS配置，例如Asset compute、Adobe Analytics和Adobe启动项。 已自动创建 **Adobe启动** 如果您使用的是AEM 6.X环境，则可以使用IMS配置或创建新的IMS配置。

已自动创建审核 **Adobe启动** 使用以下步骤配置IMS。

1. 在AEM中，打开 **工具** 菜单

1. 在安全性部分，选择Adobe IMS配置。

1. 选择 **Adobe启动** 信息卡并单击 **属性**，查看详细信息 **证书** 和 **帐户** 选项卡。 然后单击 **取消** 以在不修改任何自动创建的详细信息的情况下返回。

1. 选择 **Adobe启动** 信息卡，这次单击 **检查运行状况**，您应该会看到 **成功** 如下所示的消息。

   ![Adobe启动正常IMS配置](assets/adobe-launch-healthy-ims-config.png)


## 后续步骤

[在AEM中创建LaunchCloud Service配置](create-aem-launch-cloud-service.md)
