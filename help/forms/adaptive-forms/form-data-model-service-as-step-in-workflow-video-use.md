---
title: 在工作流中使用表单数据模型服务
seo-title: 在工作流中使用表单数据模型服务
description: 从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM工作流的一部分。 以下视频将逐步介绍在AEM工作流中配置表单数据模型步骤所需的步骤。
seo-description: 从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM工作流的一部分。 以下视频将逐步介绍在AEM工作流中配置表单数据模型步骤所需的步骤。
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: 工作流
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: 开发
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# 在工作流{#using-form-data-model-service-as-step-in-workflow}中使用表单数据模型服务作为步骤

从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM工作流的一部分。 以下视频将逐步介绍在AEM Workflow中配置表单数据模型步骤所需的步骤


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

要在服务器上测试此功能，请按照以下说明操作
* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是设置元数据属性的自定义OSGI捆绑包。
>!![NOTE]在AEM Forms 6.5及更高版本中，可以按照此处所述开箱即 [用此功能](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 如[此处](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)所述，使用SampleRest.war文件设置tomcat。Tomcat中部署的war文件具有返回申请人信用分数的代码。 信用评分是200到800之间的随机数

* [使用包管理器将资产导入AEM](assets/invoke-fdm-as-service-step.zip)。包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * 用于FDM步骤的表单数据模型。
   * 在提交时触发工作流的自适应表单。
* 打开[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 在表单提交中，将触发[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
如果信用分数超过500，该工作流会利用“或拆分”组件将应用程序路由到管理员。 如果信用评分低于500，则申请将送达
