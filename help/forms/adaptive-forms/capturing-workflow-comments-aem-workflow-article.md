---
title: 在自适应Forms Workflow中捕获工作流注释
description: 在AEM工作流中捕获工作流注释
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# 在自适应Forms Workflow中捕获工作流注释{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[仅适用于AEM Forms 6.4。在AEM Forms 6.5中，请使用变量功能来实现此用例]

常见的请求是，在电子邮件中包含任务审阅人输入的注释。 在AEM Forms 6.4中，没有现成的机制来捕获用户输入的评论并将这些评论包含在电子邮件中。

为满足此要求，提供了一个示例OSGi包，可用于捕获注释并将这些注释存储为工作流元数据属性。

以下屏幕截图显示了如何在 [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) 以捕获注释并将其存储为元数据属性。 “捕获工作流注释”是流程步骤中需要使用的java类的名称。 您需要传递将包含注释的元数据属性名称。 在下面的屏幕截图中， managerComments是用于存储注释的元数据属性。

![workflowcomments1](assets/workflowcomments1.gif)

要在系统上测试此功能，请执行以下步骤：
* [确保将工作流中的流程步骤配置为使用捕获工作流注释](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developmingwithserviceuser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 此包包含用于捕获注释并将其存储为元数据属性的示例代码

* [将与本文相关的资产下载并解压缩到您的文件系统](assets/capturecomments.zip) 资产包含工作流模型和示例自适应表单。

* 使用包管理器将2个zip文件导入AEM

* [浏览到此URL以预览表单](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填写表单字段并提交表单

* [查看您的AEM收件箱](http://localhost:4502/aem/inbox)

* 从收件箱中打开任务并提交表单。 请在出现提示时输入一些评论。

这些注释将存储在crx中名为managerComments的元数据属性中。 以管理员身份检查注释登录到crx。 工作流实例存储在以下路径中

/var/workflow/instances/server0

选择相应的工作流实例，并在元数据节点中检查属性managerComments。
