---
title: 集成 [!DNL ServiceNow]
description: 使用表单数据模型创建和显示所有事件。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# 将AEM Forms与 [!DNL ServiceNow]

在中创建和显示事件 [!DNL ServiceNow] 在AEM Forms中使用表单数据模型。

## 前提条件

* [!DNL ServiceNow] 帐户。
* 熟悉 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉 [表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 资产示例

本文提供的示例资产包括以下内容
* 云服务配置
* Swagger文件以创建事件并获取所有事件
* 基于Swagger文件的表单数据模型
* 要创建和列出的自适应表单 [!DNL ServiceNow] 事件

## 在服务器上部署资产

* 下载 [示例资产](assets/service-now.zip)
* 使用将资产导入AEM [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 编辑 [CreateIncident云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以匹配您的ServiceNow实例。
* 编辑 [GetAllIncients云服务配置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) 与ServiceNow实例匹配

## 测试集成

* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在“说明和注释”字段中输入一些值，然后单击“创建事件”按钮
* 新创建事件的事件ID应填充在文本字段中，下表应列出所有事件。
