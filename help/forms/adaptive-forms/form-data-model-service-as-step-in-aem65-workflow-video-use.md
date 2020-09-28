---
title: 在AEM 6.5工作流中将表单数据模型服务作为步骤
seo-title: 在AEM 6.5工作流中将表单数据模型服务作为步骤
description: AEM Forms6.5引入了在AEM工作流中创建变量的功能。 借助AEM工作流中的“调用表单数据模型服务”这一新功能，已变得非常简单。 以下视频将指导您完成在AEM工作流中使用调用表单数据模型服务所涉及的步骤。
seo-description: AEM Forms6.5引入了在AEM工作流中创建变量的功能。 借助AEM工作流中的“调用表单数据模型服务”这一新功能，已变得非常简单。 以下视频将指导您完成在AEM工作流中使用调用表单数据模型服务所涉及的步骤。
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# 在AEM 6.5工作流中将表单数据模型服务作为步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms6.4开始，我们现在能够将表单数据模型服务用作AEM工作流的一部分。 以下视频将逐步介绍在AEM Workflow中配置表单数据模型步骤所需的步骤

>!![NOTE]此视频演示的功能需要AEM Forms6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

要在服务器上测试此功能，请按照以下说明操作

* 如下所述，使用SampleRest. [war文件](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)设置Tomcat。Tomcat中部署的war文件具有返回申请人信用分数的代码。信用分数是200到800之间的随机数

* [ 使用包管理器将资产导入AEM](assets/aem65-loanapplication.zip)
* 该包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * 用于FDM步骤的表单数据模型。
   * 自适应表单，在提交时触发工作流。
* 打开MortgageApplication [Form](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 表单提交时，将触 [发贷款应用程序](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 工作流。

![ workflow ](assets/invokefdm651.PNG).
如果信用分数超过500，则该工作流会利用“或拆分”组件将应用程序发送给管理员。 如果信用评分低于500，则申请将送交。
