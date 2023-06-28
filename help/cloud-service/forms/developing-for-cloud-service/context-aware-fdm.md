---
title: 表单数据模型的上下文感知配置覆盖支持
description: 设置表单数据模型以根据环境与不同端点通信。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 11%

---

# 上下文感知云配置

当您在本地环境中创建云配置并在成功测试后，您会希望在上游环境中使用相同的云配置，而无需更改端点、密钥/密码和/或用户名。 要实现此用例，Cloud Service上的AEM Forms引入了定义上下文感知云配置的功能。
例如，可以通过为使用不同的连接字符串和密钥，在开发、暂存和生产环境中重用Azure存储帐户云配置。

需要执行以下步骤来创建上下文感知云配置

## 创建环境变量

可以通过 Cloud Manager 配置和管理标准环境变量。 这些变量提供给运行时环境，可以在 OSGi 配置中使用。 [根据所更改的内容，环境变量可以是特定于环境的值或环境密钥。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



以下屏幕快照显示定义的azure_key和azure_connection_string环境变量
![environment_variables](assets/environment-variables.png)

然后，可以在要在相应环境中使用的配置文件中指定这些环境变量。例如，如果您希望所有创作实例都使用这些环境变量，则您将在config.author文件夹中定义配置文件，如下所示

## 创建配置文件

在IntelliJ中打开您的项目。 导航到config.author并创建一个名为的文件

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

将以下文本复制到在上一步中创建的文件中。 此文件中的代码正在使用环境变量覆盖accountName和accountKey属性的值 **azure_connection_string** 和 **azure_key**.

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
>此配置将应用于云服务实例中的所有创作环境。 要将配置应用于发布环境，您必须将相同的配置文件放置在intelliJ项目的config.publish文件夹中
>[!NOTE]
> 请确保要覆盖的属性是云配置的有效属性。 导航到云配置以查找要覆盖的属性，如下所示。

![cloud-config-property](assets/cloud-config-properties.png)

对于使用基本身份验证的基于REST的云配置，您通常需要为serviceEndPoint、userName和密码属性创建环境变量。

## 后续步骤

[将您的AEM项目推送到Cloud Manager](./push-project-to-cloud-manager-git.md)
