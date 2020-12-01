---
title: AEM Workflow[Part2]中的变量
seo-title: AEM Workflow[Part2]中的变量
description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
seo-description: 在aem工作流中使用xml、json、arraylist、文档类型的变量
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# AEM工作流中JSON类型的变量

从AEM Forms6.5开始，我们现在可以在AEM工作流中创建JSON类型的变量。 通常，如果要根据JSON模式将自适应Forms提交到AEM工作流，或要存储表单数据模型调用操作的结果，则将创建JSON类型的变量。 以下视频将指导您完成在AEM工作流程中创建和使用JSON类型变量所需的步骤

**如果使用AEM Forms6.5.0**

创建JSON类型的变量以捕获工作流模型中提交的数据时，请勿将JSON模式与该变量关联。 这是因为提交基于JSON模式的自适应表单时，提交的数据不符合JSON模式。 JSON模式投诉数据包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms6.5.1及更高版本**

您可以在工作流模型中将模式与JSON类型的变量进行映射。 然后，您可以使用模式浏览器将模式元素与工作流模型中的字符串／数字变量进行映射

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

要使资产在您的系统上工作，请按照以下步骤操作：

* [使用包管理器下载资产并将其导入AEM](assets/jsonandstringvariable.zip)
* [浏览工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 模型，了解工作流中使用的变量
* [配置电子邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填写详细信息并提交表单
