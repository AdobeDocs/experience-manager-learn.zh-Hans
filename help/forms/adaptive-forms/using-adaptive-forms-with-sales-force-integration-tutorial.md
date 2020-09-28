---
title: 在AEM Forms6.3和6.4中使用Salesforce配置数据源
seo-title: 在AEM Forms6.3和6.4中使用Salesforce配置数据源
description: 使用表单数据模型将AEM Forms与Salesforce集成
seo-description: 使用表单数据模型将AEM Forms与Salesforce集成
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
translation-type: tm+mt
source-git-commit: cce9f5d1dae05a36b942f6b07a46c65f82eac43c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 0%

---


# 在AEM Forms6.3和6.4中使用Salesforce配置数据源{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 前提条件 {#prerequisites}

在本文中，我们将演练使用Salesforce创建数据源的过程

本教程的先决条件：

* 滚动到此页底部，下载Swagger文件并保存您的硬盘。
* AEM Forms启用SSL

   * [在AEM 6.3上启用SSL的正式文档](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [在AEM 6.4上启用SSL的正式文档](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要具有Salesforce帐户
* 您需要创建连接的应用程序。 此处列出了Salesforce用于创建应用程序的官方 [文档](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)。
* 为应用程序提供适当的OAuth范围（我已选择所有可用的OAuth范围以进行测试）
* 提供回调URL。 我的回调URL是

   * 如果您使用 **AEM Forms6.3**，则回调URL将为https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL创建潜在客户是我的表单数据模型的名称。

   * 如果您使用**AEM Forms6.4**，则回调URL将为 [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

在此示例中，gbedekar -w7-1:6443是我的服务器的名称以及AEM正在运行的端口。

创建“已连接应用程序”后，请注意 **消费方密钥和密钥**。 在AEM Forms创建数据源时，您需要这些资源。

现在，您已经创建了连接的应用程序，然后需要为需要在salesforce中执行的操作创建Swagger文件。 示例Swagger文件作为可下载资源的一部分包含在内。 此Swagger文件允许您在提交自适应表单时创建“潜在客户”对象。 请浏览此Swagger文件。

下一步是在AEM Forms创建数据源。 请按照您的AEM Forms版本执行以下步骤

## AEM Forms 6.3 {#aem-forms}

* 使用https协议登录AEM Forms
* 通过键入https://&lt;servername>:&lt;serverport> /etc/cloudservices.html导航到云服务，例如，https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下滚动到“表单数据模型”。
* 单击“显示配置”。
* 单击“+”添加新配置
* 选择“Rest Full Service”。 为配置提供有意义的标题和名称。 例如，

   * 名称：CreateLeadInSalesForce
   * 标题：CreateLeadInSalesForce

* 单击“创建”

**在下一个屏幕中**

* 选择“文件”作为Swagger源文件的选项。 浏览到之前下载的文件
* 选择身份验证类型作为OAuth2.0
* 提供ClientID和Client Secret值
* OAuth Url为- **https://login.salesforce.com/services/oauth2/authorize**
* 刷新令牌URL - **https://na5.salesforce.com/services/oauth2/token**
* **访问Url - https://na5.salesforce.com/services/oauth2/token**
* 授权范围：** api chatter_api完整id openid refresh_token visualforce web**
* 身份验证处理程序：授权承载者
* 单击“连接到OAUTH”。如果一切正常，则不会看到任何错误

使用Salesforce创建表单数据模型后，您可以使用刚刚创建的数据源创建表单数据集成。 此处是创建表单数据集成的官方 [文档](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

确保将表单数据模型配置为包含POST服务，以在SFDC中创建潜在客户对象。

您还必须为潜在客户对象配置读写服务。 请参阅本页底部的屏幕截图。

创建表单数据模型后，您可以基于此模型创建自适应Forms，并使用表单数据模型提交方法在SFDC中创建潜在客户。

## AEM Forms 6.4 {#aem-forms-1}

* 创建数据源

   * [导航到数据源](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 单击“创建”按钮
   * 提供一些有意义的值

      * 名称：CreateLeadInSalesForce
      * 标题：CreateLeadInSalesForce
      * 服务类型：RESTful服务
   * 单击“下一步”
   * Swagger来源：文件
   * 浏览并选择您在上一步中下载的Swagger文件
   * 身份验证类型：OAuth 2.0。指定以下值
   * 提供ClientID和Client Secret值
   * OAuth Url为- **https://login.salesforce.com/services/oauth2/authorize**
   * 刷新令牌URL - **https://na5.salesforce.com/services/oauth2/token**
   * 访问令牌&#x200B;**Url - https://na5.salesforce.com/services/oauth2/token**
   * 授权范围：** api chatter_api完整id openid refresh_token visualforce web**
   * 身份验证处理程序：授权承载者
   * 单击“连接到OAuth”按钮。 如果您看到任何错误，请查看上述步骤，确保准确输入所有信息。


使用SalesForce创建数据源后，您可以使用刚刚创建的数据源创建表单数据集成。 此处是该文档链 [接](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

确保将表单数据模型配置为包含POST服务，以在SFDC中创建潜在客户对象。

您还必须为潜在客户对象配置读写服务。 请参阅本页底部的屏幕截图。

创建表单数据模型后，您可以基于此模型创建自适应Forms，并使用表单数据模型提交方法在SFDC中创建潜在客户。

>[!NOTE]
>
>确保Swagger文件中的url与您所在的区域相对应。 例如，示例Swagger文件中的url为“na46.salesforce.com”，因为该帐户是在北美创建的。 最简单的方法是登录您的Salesforce帐户并检查url。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
