---
title: 在AEM 6.5工作流中将表单数据模型服务用作步骤
description: AEM Forms 6.5引入了在AEM Workflow中创建变量的功能。 借助这项新功能，在AEM Workflow中使用“调用表单数据模型服务”变得非常容易。 以下视频将指导您完成在AEM Workflow中使用调用表单数据模型服务所涉及的步骤。
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# 在AEM 6.5工作流中将表单数据模型服务用作步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms 6.4开始，我们现在能够将表单数据模型服务用作AEM Workflow的一部分。 以下视频介绍了在AEM Workflow中配置表单数据模型步骤所需的步骤

>!![NOTE]本视频中演示的功能需要AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

要在您的服务器上测试此功能，请按照以下说明操作

* 使用SampleRest.war文件设置tomcat，如所述 [此处](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)部署在Tomcat中的war文件具有返回申请人的信用分数的代码。信用分数是200到800之间的随机数

* [ 使用包管理器将资源导入AEM](assets/aem65-loanapplication.zip)
* 该软件包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * FDM步骤中使用的表单数据模型。
   * 自适应表单，用于在提交时触发工作流。
* 打开 [MortageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填写详细信息并提交。 在提交的表单上 [贷款应用工作流](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 触发。

![ 工作流 ](assets/invokefdm651.PNG).
如果信用评分超过500，工作流会利用“或分解”组件将申请传送给管理员。 如果信用评分低于500，则申请将被传送至cavery。
