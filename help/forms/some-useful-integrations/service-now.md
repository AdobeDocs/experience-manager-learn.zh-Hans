---
title: 与 [!DNL ServiceNow]集成
description: 使用表单数据模型创建和显示所有事件。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# 将AEM Forms与[!DNL ServiceNow]集成

在[!DNL ServiceNow]中使用AEM Forms中的表单数据模型创建和显示事件。

## 先决条件

* [!DNL ServiceNow]帐户。
* 熟悉[创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=zh-Hans)
* 熟悉[表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=zh-Hans)

## Assets示例

本文提供的示例资源包括

* 云服务配置
* 使用Swagger文件创建事件并获取所有   事件
* 基于swagger文件的表单数据模型
* 用于创建和列出[!DNL ServiceNow]事件的自适应表单

## 在服务器上部署资源

* 下载[示例资源](assets/service-now.zip)
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)将资源导入AEM
* 用于此集成的swagger文件位于crx存储库的```/conf/9957/settings/cloudconfigs/fdm```文件夹下
* 编辑[CreateIncident云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以匹配您的ServiceNow实例。
* 编辑[GetAllIncidents云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents)以匹配您的ServiceNow实例。 您需要更改主机、用户名和密码，以匹配您的ServiceNow实例凭据。

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
