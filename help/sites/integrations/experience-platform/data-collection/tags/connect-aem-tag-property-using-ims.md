---
title: 使用IMS将AEM Sites与标记属性连接
description: 了解如何使用AEM中的IMS配置将AEM Sites与标记属性连接。
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
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# 使用IMS将AEM Sites与标记属性连接{#connect-aem-and-tag-property-using-ims}

了解如何使用AEM中的IMS (Identity Management System)配置将AEM与标记属性连接。 此设置使用标记API验证AEM的身份，并允许AEM通过标记API进行通信以访问标记属性。

## 创建或重用IMS配置

需要使用Adobe Developer Console项目的IMS配置将AEM与新创建的标记属性集成。 此配置允许AEM使用标记API与标记应用程序进行通信，并且IMS处理此集成的安全方面。

每当配置AEM as aCloud Service环境时，都会自动创建一些IMS配置，例如Asset compute、Adobe Analytics和标记。 如果您使用AEM 6.X环境，则可以使用Adobe Experience Platform **IMS配置中自动创建的**&#x200B;标记或应创建新的IMS配置。

使用以下步骤查看在Adobe Experience Platform **IMS配置中自动创建的**&#x200B;标记。

1. 在AEM Author中，打开&#x200B;**工具**&#x200B;菜单
1. 在安全性部分，选择Adobe IMS配置。
1. 选择&#x200B;**Adobe启动项**&#x200B;卡并单击&#x200B;**属性**，从&#x200B;**证书**&#x200B;和&#x200B;**帐户**&#x200B;选项卡中查看详细信息。 然后单击&#x200B;**取消**&#x200B;返回而不修改任何自动创建的详细信息。
1. 选择&#x200B;**Adobe启动项**&#x200B;信息卡，这次单击&#x200B;**检查运行状况**，您应该会看到如下所示的&#x200B;**成功**&#x200B;消息。

   ![标记正常IMS配置](assets/adobe-launch-healthy-ims-config.png)

## 后续步骤

[在AEM中创建标记Cloud Service配置](create-aem-launch-cloud-service.md)
