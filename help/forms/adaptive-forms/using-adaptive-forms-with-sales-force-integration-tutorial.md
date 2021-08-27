---
title: 在AEM Forms 6.3和6.4中使用Salesforce配置DataSource
description: 使用表单数据模型将AEM Forms与Salesforce集成
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 0%

---


# 在AEM Forms 6.3和6.4中使用Salesforce配置DataSource{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 前提条件 {#prerequisites}

在本文中，我们将介绍使用Salesforce创建数据源的过程

本教程的先决条件：

* 滚动到此页面底部，下载swagger文件并保存硬盘。
* 启用SSL的AEM Forms

   * [有关在AEM 6.3上启用SSL的正式文档](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [有关在AEM 6.4上启用SSL的正式文档](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您将需要拥有Salesforce帐户
* 您需要创建一个连接的应用程序。 用于创建应用程序的Salesforce官方文档在[此处](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)列出。
* 为应用程序提供相应的OAuth作用域（我已选择所有可用的OAuth作用域以进行测试）
* 提供回调URL。 以我为例的回调URL是

   * 如果您使用的是&#x200B;**AEM Forms 6.3**，则回调URL将为https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL中，创建潜在客户是我的表单数据模型的名称。

   * 如果您使用的是** AEM Forms 6.4**，则回调URL将为https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

在本示例中， gbedekar -w7-1:6443是我的服务器名称以及运行AEM的端口。

创建“连接的应用程序”后，请注意&#x200B;**使用者密钥和密钥**。 在AEM Forms中创建数据源时，您将需要使用这些参数。

现在，您已创建连接的应用程序，接下来将需要为需要在salesforce中执行的操作创建swagger文件。 样例swagger文件将作为可下载资产的一部分包含在内。 利用此swagger文件，可在提交自适应表单时创建“潜在客户”对象。 请浏览此swagger文件。

下一步是在AEM Forms中创建数据源。 请根据您的AEM Forms版本执行以下步骤

## AEM Forms 6.3 {#aem-forms}

* 使用https协议登录AEM Forms
* 通过键入https://&lt;servername>:&lt;serverport> /etc/cloudservices.html导航到云服务，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下滚动到“表单数据模型”。
* 单击“显示配置”。
* 单击“+”以添加新配置
* 选择“Rest Full Service”。 为配置提供有意义的标题和名称。 例如，

   * 名称：CreateLeadInSalesForce
   * 标题：CreateLeadInSalesForce

* 单击“创建”

**在下一个屏幕**

* 选择“文件”作为Swagger源文件的选项。 浏览到您之前下载的文件
* 选择身份验证类型作为OAuth2.0
* 提供ClientID和客户端密钥值
* Oauth Url为 — **https://login.salesforce.com/services/oauth2/authorize**
* 刷新令牌Url - **https://na5.salesforce.com/services/oauth2/token**
* **访问Toke Url - https://na5.salesforce.com/services/oauth2/token**
* 授权范围：** api   chatter_api完整id   openid   refresh_token visualforce web**
* 身份验证处理程序：授权承载者
* 单击“连接到OAUTH”。如果一切正常，您不应看到任何错误

使用Salesforce创建表单数据模型后，您可以使用刚刚创建的数据源创建表单数据集成。 有关创建表单数据集成的正式文档为[此处](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

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
   * 单击下一步
   * Swagger源：文件
   * 浏览并选择您在上一步骤中下载的swagger文件
   * 身份验证类型：OAuth 2.0。指定以下值
   * 提供ClientID和客户端密钥值
   * Oauth Url为 — **https://login.salesforce.com/services/oauth2/authorize**
   * 刷新令牌Url - **https://na5.salesforce.com/services/oauth2/token**
   * 访问令牌Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授权范围：** api chatter_api完整id openid refresh_token visualforce web**
   * 身份验证处理程序：授权承载者
   * 单击“连接到OAuth”按钮。 如果您发现任何错误，请查看上述步骤，以确保准确输入所有信息。


使用SalesForce创建数据源后，您可以使用刚刚创建的数据源创建表单数据集成。 其文档链接为[此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

确保将表单数据模型配置为包含POST服务，以在SFDC中创建潜在客户对象。

您还必须为潜在客户对象配置读写服务。 请参阅本页底部的屏幕截图。

创建表单数据模型后，您可以基于此模型创建自适应Forms，并使用表单数据模型提交方法在SFDC中创建潜在客户。

>[!NOTE]
>
>确保swagger文件中的url与您所在的区域相对应。 例如，在北美地区创建帐户时，swagger示例文件中的url为“na46.salesforce.com”。 最简单的方法是登录Salesforce帐户并检查url 。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
