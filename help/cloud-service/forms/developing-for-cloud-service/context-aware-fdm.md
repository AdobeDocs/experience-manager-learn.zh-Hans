---
title: 表单数据模型的上下文感知配置覆盖支持
description: 设置表单数据模型以根据环境与不同端点通信。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
duration: 80
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 7%

---

# 上下文感知云配置

在本地环境中创建云配置并成功测试后，您将希望在上游环境中使用相同的云配置，而无需更改端点、密钥/密码和/或用户名。 为了实现此用例，Cloud Service上的AEM Forms引入了定义上下文感知云配置的功能。
例如，可以通过为使用不同的连接字符串和密钥，在开发、暂存和生产环境中重用Azure存储帐户云配置。

需要执行以下步骤来创建上下文感知云配置

## 创建环境变量

可以通过 Cloud Manager 配置和管理标准环境变量。这些变量提供给运行时环境，可以在 OSGi 配置中使用。[根据所更改的内容，环境变量可以是特定于环境的值或环境密钥。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



以下屏幕截图显示了定义的azure_key和azure_connection_string环境变量
![environment_variables](assets/environment-variables.png)

然后，可以在配置文件中指定这些环境变量以在相应的环境中使用
例如，如果您希望所有创作实例都使用这些环境变量，则将在config.author文件夹中定义配置文件，如下所示

## 创建配置文件

在IntelliJ中打开您的项目。 导航到config.author并创建一个名为的文件

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

将以下文本复制到上一步中创建的文件中。 此文件中的代码正在使用环境变量&#x200B;**azure_connection_string**&#x200B;和&#x200B;**azure_key**&#x200B;覆盖accountName和accountKey属性的值。

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>此配置将应用于云服务实例中的所有创作环境。 要将配置应用于发布环境，必须将相同的配置文件放置在intelliJ项目的config.publish文件夹中
>[!NOTE]
> 请确保要覆盖的属性是云配置的有效属性。 导航到云配置以查找要覆盖的属性，如下所示。

![云配置属性](assets/cloud-config-properties.png)

对于使用基本身份验证的基于REST的云配置，您通常希望为serviceEndPoint、userName和密码属性创建环境变量。

## 后续步骤

[将AEM项目推送到Cloud Manager](./push-project-to-cloud-manager-git.md)
