---
title: 在工作流中以表单数据模型服务为步骤
seo-title: 在工作流中以表单数据模型服务为步骤
description: 从AEM Forms6.4开始，我们现在能够将表单数据模型作为AEM工作流的一部分使用。 以下视频将逐步介绍在AEM Workflow中配置表单数据模型步骤所需的步骤。
seo-description: 从AEM Forms6.4开始，我们现在能够将表单数据模型作为AEM工作流的一部分使用。 以下视频将逐步介绍在AEM Workflow中配置表单数据模型步骤所需的步骤。
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# 在工作流中以表单数据模型服务为步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms6.4开始，我们现在能够将表单数据模型作为AEM工作流的一部分使用。 以下视频将逐步介绍在AEM Workflow中配置表单数据模型步骤所需的步骤


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

要在服务器上测试此功能，请按照以下说明操作
* [下载并部署setvalue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是设置元数据属性的自定义OSGI包。
>!![NOTE]在AEM Forms6.5及更高版本中，此功能现成可用，如 [下所述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 如下所述，使用SampleRest.war [文件](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)设置tomcat。Tomcat中部署的war文件具有返回申请人信用分数的代码。 信用评分是200到800之间的随机数

* [使用包管理器将资产导入AEM](assets/invoke-fdm-as-service-step.zip)。包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * 用于FDM步骤的表单数据模型。
   * 自适应表单，在提交时触发工作流。
* 打开MortgageApplication [Form](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 表单提交时，将触 [发贷款应用程序](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 工作流。

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
如果信用分数超过500，则该工作流会利用“或拆分”组件将应用程序发送给管理员。 如果信用评分低于500，则申请将送交
