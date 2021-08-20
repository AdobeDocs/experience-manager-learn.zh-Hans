---
title: AEM Workflow[Part2]中的变量
description: 在AEM工作流中使用XML、JSON、ArrayList和Document类型的变量
version: 6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# AEM工作流中JSON类型的变量

从AEM Forms 6.5开始，我们现在可以在AEM工作流中创建JSON类型的变量。 如果您要基于JSON架构将自适应Forms提交到AEM工作流，或者要存储表单数据模型调用操作的结果，通常会创建JSON类型的变量。 以下视频将指导您完成在AEM工作流中创建和使用JSON类型变量所需的步骤

**如果使用AEM Forms 6.5.0**

创建JSON类型的变量以在工作流模型中捕获提交的数据时，请不要将JSON架构与变量关联。 这是因为在提交基于JSON模式的自适应表单时，提交的数据与JSON模式不兼容。 JSON架构投诉数据将包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms 6.5.1及更高版本**

您可以在工作流模型中使用JSON类型的变量映射架构。 然后，您可以使用架构浏览器将架构元素与工作流模型中的字符串/数字变量进行映射

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

要使资产在您的系统上工作，请执行以下步骤：

* [使用包管理器下载资产并将其导入AEM](assets/jsonandstringvariable.zip)
* [浏览工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 模型，以了解工作流中使用的变量
* [配置电子邮件服务](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填写详细信息并提交表格
