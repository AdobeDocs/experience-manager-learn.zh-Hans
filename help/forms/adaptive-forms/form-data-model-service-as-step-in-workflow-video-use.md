---
title: 将表单数据模型服务用作工作流中的步骤
description: 从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM Workflow的一部分。 以下视频介绍了在AEM Workflow中配置表单数据模型步骤所需的步骤。
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# 将表单数据模型服务用作工作流中的步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms 6.4开始，我们现在能够将表单数据模型用作AEM Workflow的一部分。 以下视频介绍了在AEM Workflow中配置表单数据模型步骤所需的步骤


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

要在您的服务器上测试此功能，请按照以下说明操作
* [下载并部署setvalue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是用于设置元数据属性的自定义OSGI捆绑包。
>!![NOTE]在AEM Forms 6.5及更高版本中，此功能可开箱即用，例如 [在此处描述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用SampleRest.war文件设置tomcat，如所述 [此处](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)在Tomcat中部署的war文件具有返回申请人的信用分数的代码。 信用评分是200到800之间的随机数

* [使用包管理器将资源导入AEM](assets/invoke-fdm-as-service-step.zip).该软件包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * FDM步骤中使用的表单数据模型。
   * 自适应表单，用于在提交时触发工作流。
* 打开 [MortageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填写详细信息并提交。 在提交的表单上 [贷款应用工作流](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 触发。

![ 工作流 ](assets/fdm-as-service-step-workflow.PNG).
如果信用评分超过500，工作流会利用“或分解”组件将申请传送给管理员。 如果信用评分低于500，则申请将被传送至cavery
