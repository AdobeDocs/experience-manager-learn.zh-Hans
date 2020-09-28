---
title: 使用表单数据模型发布二进制数据
seo-title: 使用表单数据模型发布二进制数据
description: 使用表单数据模型将二进制数据发布到AEM DAM
seo-description: 使用表单数据模型将二进制数据发布到AEM DAM
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---


# 使用表单数据模型发布二进制数据{#using-form-data-model-to-post-binary-data}

从AEM Forms6.4开始，我们现在能够调用表单数据模型服务作为AEM工作流程中的一个步骤。 本文将引导您了解使用表单数据模型服务发布记录文档的示例用例。

用例如下：

1. 用户填写并提交自适应表单。
1. 自适应表单被配置为生成记录文档。
1. 提交此自适应表单时，将触发AEM工作流，它将使用调用表单数据模型服务来POST记录文档到AEM DAM。

![posttodam](assets/posttodamshot1.png)

表单数据模型选项卡——属性

在“服务输入”选项卡中，我们映射以下内容：

* 文件（需要存储的二进制对象），其属性为与有效负荷相关的DOR.pdf。 这意味着，当提交自适应表单时，生成的记录文档将存储在一个名为DOR.pdf的文件中，该文件与工作流有效负荷相关。**确保此DOR.pdf与配置自适应表单的提交属性时提供的相同。**

* fileName —— 这是二进制对象在DAM中存储时使用的名称。 因此，您希望动态生成此属性，以便每个fileName在提交时都是唯一的。 为此，我们使用工作流中的流程步骤创建了名为filename的元数据属性，并将其值设置为提交表单的人员的成员名称和帐户编号的组合。 例如，如果此人的成员姓名是John Jacobs，而他的帐号是9846，则文件名将是John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服务输入

>[!NOTE]
>
>故障排除提示——如果由于某种原因未在DAM中创建DOR.pdf，请单击此处重置数据源身份验证 [设置](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)。 这些是AEM身份验证设置，默认为admin/admin。

要在服务器上测试此功能，请按照以下步骤操作：

1.部[署使用serviceuser包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下载并部署setvalue bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此自定义OSGI bundle用于创建元数据属性并从提交的表单数据设置其值。

1. [使用包管理器](assets/postdortodam.zip) ，将与本文关联的资产导入AEM。您将获得以下信息：

   1. 工作流模型
   1. 自适应表单配置为提交到AEM工作流
   1. 配置为使用PostToDam.JSON文件的数据源
   1. 使用数据源的表单数据模型

1. 指向浏 [览器以打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填写表单并提交。
1. 如果创建并存储了记录文档，请检查资产应用程序。


[创建数据源](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 时使用的Swagger文件可供您参考
