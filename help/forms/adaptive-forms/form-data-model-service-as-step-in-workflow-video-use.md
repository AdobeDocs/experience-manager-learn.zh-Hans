---
title: 在工作流中使用表单数据模型服务作为步骤
description: 从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM工作流的一部分。 以下视频将演示在AEM工作流中配置表单数据模型步骤所需的步骤。
feature: 工作流
type: Tutorial
version: 6.4,6.5
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 在工作流中使用表单数据模型服务作为步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM工作流的一部分。 以下视频将演示在AEM工作流中配置表单数据模型步骤所需的步骤


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

要在服务器上测试此功能，请按照以下说明操作
* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是用于设置元数据属性的自定义OSGI包。
>!![NOTE]在AEM Forms 6.5及更高版本中，此功能可开箱即用，如 [下所述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 如[此处](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)所述，使用SampleRest.war文件设置tomcat。Tomcat中部署的war文件具有返回申请人信用得分的代码。 信用得分是200到800之间的随机数

* [使用包管理器将资产导入AEM](assets/invoke-fdm-as-service-step.zip)。该包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * 在FDM步骤中使用的表单数据模型。
   * 自适应表单，在提交时触发工作流。
* 打开[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 在提交表单时，将触发[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ workflow ](assets/fdm-as-service-step-workflow.PNG).
如果信用分数超过500，则工作流会利用或拆分组件将应用程序路由到管理员。 如果信用分数小于500，则将申请路由到付款
