---
title: AEM Workflow[第2部分]中的变量
description: 在AEM工作流中使用XML、JSON、ArrayList、Document类型的变量
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# AEM Workflow中的JSON类型变量

从AEM Forms 6.5开始，我们现在可以在AEM Workflow中创建JSON类型的变量。 通常，如果您将基于JSON架构的自适应Forms提交到AEM Workflow，或者希望存储表单数据模型调用操作的结果，则您将创建JSON类型的变量。 以下视频将指导您完成在AEM工作流中创建和使用JSON类型变量所需的步骤

**如果使用AEM Forms 6.5.0**

创建类型为JSON的变量以在工作流模型中捕获提交的数据时，请勿将JSON架构与变量关联。 这是因为在提交基于JSON架构的自适应表单时，提交的数据不符合JSON架构。 JSON架构投诉数据包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms 6.5.1及更高版本**

您可以在工作流模型中使用类型为JSON的变量映射架构。 然后，您可以使用架构浏览器将架构元素映射到工作流模型中的字符串/数字变量

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

要使资源在系统中正常工作，请执行以下步骤：

* [使用包管理器下载资源并将其导入AEM](assets/jsonandstringvariable.zip)
* [浏览工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html)以了解工作流中使用的变量
* [配置电子邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填写详细信息并提交表单
