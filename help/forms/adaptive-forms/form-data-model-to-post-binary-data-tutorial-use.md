---
title: 使用表单数据模型发布二进制数据
description: 使用表单数据模型将二进制数据发布到AEM DAM
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 使用表单数据模型发布二进制数据{#using-form-data-model-to-post-binary-data}

从AEM Forms 6.4开始，我们现在能够在AEM Workflow中作为步骤调用表单数据模型服务。 本文将介绍使用表单数据模型服务发布记录文档的示例用例。

用例如下所示：

1. 用户填写并提交自适应表单。
1. 自适应表单配置为生成记录文档。
1. 提交此自适应表单时，会触发AEM Workflow，它将使用调用表单数据模型服务将记录文档POST到AEM DAM。

![posttodam](assets/posttodamshot1.png)

表单数据模型选项卡 — 属性

在“服务输入”选项卡中，我们映射以下内容

* 相对于有效负荷具有DOR.pdf属性的文件（需要存储的二进制对象）。 这意味着，在提交自适应表单时，生成的记录文档将存储在名为DOR.pdf的文件中，该文件与工作流有效负载相关。**确保此DOR.pdf与您在配置自适应表单的提交属性时提供的相同。**

* fileName — 这是在DAM中存储二进制对象时所使用的名称。 因此，您希望动态生成此属性，以便每个提交的fileName都是唯一的。 为此，我们使用工作流中的流程步骤来创建名为filename的元数据属性，并将其值设置为提交表单人员的成员名称和帐号的组合。 例如，如果人员的成员名为John Jacobs，帐号为9846，则文件名为John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服务输入

>[!NOTE]
>
>故障排除提示 — 如果由于某种原因未在DAM中创建DOR.pdf，请通过单击重置数据源身份验证设置 [此处](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). 这些是AEM身份验证设置，默认情况下为admin/admin。

要在您的服务器上测试此功能，请按照以下所述步骤操作：

1.[部署Developingwithserviceuser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下载并部署setvalue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).此自定义OSGI捆绑包用于创建元数据属性，并从提交的表单数据设置其值。

1. [导入资源](assets/postdortodam.zip) 使用包管理器将与此文章关联到AEM中。您将获得以下内容

   1. 工作流模型
   1. 配置为提交到AEM Workflow的自适应表单
   1. 配置为使用PostToDam.JSON文件的数据源
   1. 使用数据源的表单数据模型

1. 指向您的 [浏览器以打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填写表单并提交。
1. 如果创建并存储了记录文档，请选中Assets应用程序。


[Swagger文件](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 用于创建数据源可供您参考
