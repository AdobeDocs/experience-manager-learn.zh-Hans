---
title: 在AEM 6.5工作流中使用表单数据模型服务作为步骤
description: AEM Forms 6.5引入了在AEM工作流中创建变量的功能。 使用AEM工作流中的“调用表单数据模型服务”这一新功能变得非常简单。 以下视频将指导您完成在AEM工作流中使用调用表单数据模型服务涉及的步骤。
feature: 工作流
type: Tutorial
version: 6.5.
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 1%

---


# 在AEM 6.5工作流中使用表单数据模型服务作为步骤 {#using-form-data-model-service-as-step-in-workflow}

从AEM Forms 6.4开始，我们现在能够将表单数据模型服务用作AEM工作流的一部分。 以下视频将演示在AEM工作流中配置表单数据模型步骤所需的步骤

>!![NOTE]此视频中演示的功能需要AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

要在服务器上测试此功能，请按照以下说明操作

* 如[此处](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)所述，使用SampleRest.war文件设置tomcat。Tomcat中部署的war文件具有返回申请人信用得分的代码。信用得分是200到800之间的随机数

* [ 使用包管理器将资产导入AEM](assets/aem65-loanapplication.zip)
* 该包包含以下内容：

   * 使用FDM步骤的工作流模型。
   * 在FDM步骤中使用的表单数据模型。
   * 自适应表单，在提交时触发工作流。
* 打开[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填写详细信息并提交。 在提交表单时，将触发[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ workflow ](assets/invokefdm651.PNG).
如果信用分数超过500，则工作流会利用或拆分组件将应用程序路由到管理员。 如果信用分数小于500，则申请将被路由到付款。
