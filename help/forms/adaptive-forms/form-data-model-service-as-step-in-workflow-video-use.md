---
title: 在工作流中将表单数据模型服务用作步骤
description: 从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM Workflow的一部分。 以下视频介绍了在AEM Workflow中配置表单数据模型步骤所需的步骤。
feature: Workflow
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 在工作流中将表单数据模型服务用作步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM Workflow的一部分。 以下视频介绍了在AEM工作流中配置表单数据模型步骤所需的步骤


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

要在您的服务器上测试此功能，请按照以下说明操作
* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是用于设置元数据属性的自定义OSGI捆绑包。
>在AEM Forms 6.5及更高版本中，此功能现成可用，如[此处所述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用[此处](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=zh-Hans)所述的SampleRest.war文件设置tomcat。在Tomcat中部署的war文件具有返回申请人的信用分数的代码。 信用得分是200到800之间的随机数

* [使用包管理器将资源导入AEM](assets/invoke-fdm-as-service-step.zip)。该包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * FDM步骤中使用的表单数据模型。
   * 自适应表单在提交时触发工作流。
* 打开[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 在提交表单时，触发[贷款申请工作流](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![工作流](assets/fdm-as-service-step-workflow.PNG)。
如果信用评分超过500，工作流将利用“或拆分”组件将申请路由给管理员。 如果信用评分低于500，则申请将被转给认证机构
