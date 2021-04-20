---
title: 使用表单数据模型发布二进制数据
seo-title: 使用表单数据模型发布二进制数据
description: 使用表单数据模型将二进制数据发布到AEM DAM
seo-description: 使用表单数据模型将二进制数据发布到AEM DAM
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 1%

---


# 使用表单数据模型发布二进制数据{#using-form-data-model-to-post-binary-data}

从AEM Forms 6.4开始，我们现在可以调用表单数据模型服务，这是AEM工作流中的一个步骤。 本文将引导您了解使用表单数据模型服务发布记录文档的示例用例。

用例如下：

1. 用户填写并提交自适应表单。
1. 自适应表单配置为生成记录文档。
1. 提交此自适应表单时，将触发AEM工作流，该工作流将使用调用表单数据模型服务将记录文档POST到AEM DAM。

![posttodam](assets/posttodamshot1.png)

表单数据模型选项卡 — 属性

在“服务输入”选项卡中，我们映射以下内容

* 文件（需要存储的二进制对象），其DOR.pdf属性相对于有效负荷。 这意味着当提交自适应表单时，生成的记录文档将存储在一个名为DOR.pdf的文件中，该文件与工作流有效负荷相关。**确保此DOR.pdf与配置自适应表单的提交属性时提供的相同。**

* fileName — 这是二进制对象在DAM中存储时使用的名称。 因此，您希望动态生成此属性，以便每个fileName在每次提交时都是唯一的。 为此，我们使用工作流中的流程步骤创建了名为filename的元数据属性，并将其值设置为提交表单的人员的成员名称和帐户编号的组合。 例如，如果此人的成员姓名是John Jacobs，其帐号是9846，则文件名是John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服务输入

>[!NOTE]
>
>拍摄提示 — 如果由于某种原因未在DAM中创建DOR.pdf，请通过单击[此处](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)重置数据源身份验证设置。 这些是AEM身份验证设置，默认情况下为admin/admin。

要在服务器上测试此功能，请按照以下步骤操作：

1.[使用服务用户包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)部署开发

1. [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此自定义OSGI包用于创建元数据属性并从提交的表单数据设置其值。

1. [使用包](assets/postdortodam.zip) 管理器将与本文关联的资产导入AEM。您将获得以下内容

   1. 工作流模型
   1. 自适应表单配置为提交到AEM工作流
   1. 配置为使用PostToDam.JSON文件的数据源
   1. 使用数据源的表单数据模型

1. 指向您的[浏览器以打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填写表单并提交。
1. 如果创建并存储了记录文档，请选中“资产”应用程序。


[创建](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 数据源时使用的Swagger文件可供参考
