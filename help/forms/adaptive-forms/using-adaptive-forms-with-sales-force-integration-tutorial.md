---
title: 在AEM Forms 6.3和6.4中使用Salesforce配置数据源
description: 使用表单数据模型将AEM Forms与Salesforce集成
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 175
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# 在AEM Forms 6.3和6.4中使用Salesforce配置数据源{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 先决条件 {#prerequisites}

在本文中，我们将介绍使用Salesforce创建数据Source的过程

本教程的先决条件：

* 滚动到本页底部，下载swagger文件并将其保存到硬盘上。
* 启用了SSL的AEM Forms

   * [有关在AEM 6.3上启用SSL的官方文档](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [有关在AEM 6.4上启用SSL的官方文档](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要拥有Salesforce帐户
* 您需要创建连接的应用程序。 [此处](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)列出了用于创建应用程序的官方文档表单Salesforce。
* 为应用程序提供适当的OAuth范围（出于测试目的，我已选择所有可用的OAuth范围）
* 提供回调URL。 我案例中的回调URL是

   * 如果您使用的是&#x200B;**AEM Forms 6.3**，则回调URL为https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL中， createlead是我的表单数据模型的名称。

   * 如果您使用&#x200B;**AEM Forms 6.4**，则回调URL为https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

在此示例中，gbedekar -w7-1:6443是AEM所运行服务器和端口的名称。

创建连接的应用程序后，请记下&#x200B;**使用者密钥和密钥**。 在AEM Forms中创建数据源时，您需要这些组件。

现在您已经创建了连接的应用程序，接下来需要为需要在salesforce中执行的操作创建一个swagger文件。 示例swagger文件作为可下载资源的一部分包含在内。 此swagger文件允许您在提交自适应表单时创建“Lead”对象。 请浏览此swagger文件。

下一步是在AEM Forms中创建数据Source。 请根据您的AEM Forms版本执行以下步骤

## AEM Forms 6.3 {#aem-forms}

* 使用https协议登录AEM Forms
* 键入https://&lt;servername>：&lt;serverport> /etc/cloudservices.html以导航到云服务，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下滚动至“表单数据模型”。
* 单击“显示配置”。
* 单击“+”以添加新配置
* 选择“Rest Full Service”。 为配置提供有意义的标题和名称。 例如，

   * 名称：CreateLeadInSalesForce
   * Title： CreateLeadInSalesForce

* 单击“创建”

**在下一个屏幕中**

* 选择“文件”作为Swagger源文件的选项。 浏览到您之前下载的文件
* 选择身份验证类型作为OAuth2.0
* 提供ClientID和客户端密钥值
* OAuth Url为 — **https://login.salesforce.com/services/oauth2/authorize**
* 刷新令牌Url - **https://na5.salesforce.com/services/oauth2/token**
* **访问Toke Url - https://na5.salesforce.com/services/oauth2/token**
* 授权范围： **&#x200B; api   chatter_api完整id   openid   refresh_token visualforce web**
* 身份验证处理程序：授权载体
* 单击“连接到OAUTH”。如果一切顺利，您应该不会看到任何错误

使用Salesforce创建表单数据模型后，您可以使用刚刚创建的数据Source创建表单数据集成。 有关创建表单数据集成的官方文档为[此处](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)。

确保将表单数据模型配置为包含POST服务，以便在SFDC中创建Lead对象。

您还必须为Lead对象配置读写服务。 请参阅本页底部的屏幕截图。

创建表单数据模型后，您可以根据此模型创建自适应Forms，并使用表单数据模型提交方法在SFDC中创建Lead。

## AEM Forms 6.4 {#aem-forms-1}

* 创建数据Source

   * [导航到数据源](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 单击“创建”按钮
   * 提供一些有意义的值

      * 名称：CreateLeadInSalesForce
      * Title： CreateLeadInSalesForce
      * 服务类型： RESTful服务

   * 单击“下一步”
   * Swagger Source：文件
   * 浏览并选择您在上一步中下载的swagger文件
   * 身份验证类型：OAuth 2.0。指定以下值
   * 提供ClientID和客户端密钥值
   * OAuth Url为 — **https://login.salesforce.com/services/oauth2/authorize**
   * 刷新令牌Url - **https://na5.salesforce.com/services/oauth2/token**
   * 访问令牌Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授权范围： **&#x200B; api chatter_api完整id openid refresh_token visualforce web**
   * 身份验证处理程序：授权载体
   * 单击“连接到OAuth”按钮。 如果您看到任何错误，请查看上述步骤以确保正确输入了所有信息。

使用Salesforce创建数据Source后，您可以使用刚刚创建的数据Source创建表单数据集成。 其文档链接位于[此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

确保将表单数据模型配置为包含POST服务，以便在SFDC中创建Lead对象。

您还必须为Lead对象配置读写服务。 请参阅本页底部的屏幕截图。

创建表单数据模型后，您可以根据此模型创建自适应Forms，并使用表单数据模型提交方法在SFDC中创建Lead。

>[!NOTE]
>
>确保swagger文件中的url对应于您所在的地区。 例如，示例swagger文件中的url为“na46.salesforce.com”，因为该帐户是在北美创建的。 最简单的方法是登录到您的Salesforce帐户并检查URL 。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
