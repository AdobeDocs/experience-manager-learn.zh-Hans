---
title: 集成 [!DNL ServiceNow]
description: 使用表单数据模型创建和显示所有事件。
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 56
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# 将AEM Forms与集成 [!DNL ServiceNow]

在中创建和显示事件 [!DNL ServiceNow] 使用AEM Forms中的表单数据模型。

## 前提条件

* [!DNL ServiceNow] 帐户。
* 熟悉 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉 [表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 示例资源

本文提供的示例资源包括

* 云服务配置
* Swagger文件用于创建事件并获取所有事件
* 基于swagger文件的表单数据模型
* 要创建并列出的自适应表单 [!DNL ServiceNow] 事件

## 在服务器上部署资源

* 下载 [示例资源](assets/service-now.zip)
* 使用将资源导入AEM [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 用于此集成的swagger文件位于 ```/conf/9957/settings/cloudconfigs/fdm``` crx存储库中的文件夹
* 编辑 [CreateIncident云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以匹配您的ServiceNow实例。
* 编辑 [GetAllIncidents云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) 以匹配您的ServiceNow实例。 您需要更改主机、用户名和密码，以匹配您的ServiceNow实例凭据。

## 访问ServiceNow实例凭据

* 单击您的用户配置文件
  ![单击用户配置文件](assets/snow-1.png)

* 单击管理实例密码
* 实例详细信息如下所示
  ![实例详细信息](assets/snow-3.png)

## 测试集成

* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在说明和备注字段中输入一些值，然后单击“创建意外事件”按钮
* 应在文本字段中填充新创建的事件的事件ID，下表应列出所有事件。
