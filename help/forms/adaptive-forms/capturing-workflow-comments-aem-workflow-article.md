---
title: 在自适应Forms Workflow中捕获工作流注释
description: 在AEM Workflow中捕获工作流注释
feature: Workflow
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# 在自适应Forms Workflow中捕获工作流注释{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[仅适用于AEM Forms 6.4。在AEM Forms 6.5中，请使用变量功能来实现此用例]

常见的请求是能够在电子邮件中包含任务审阅者输入的注释。 在AEM Forms 6.4中，没有现成的机制来捕获用户输入的注释并在电子邮件中包含这些注释。

为了满足此要求，提供了一个示例OSGi捆绑包，该捆绑包可用于捕获注释并将这些注释存储为工作流元数据属性。

以下屏幕截图显示了如何使用[AEM工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)中的进程步骤来捕获注释并将其存储为元数据属性。 “捕获工作流注释”是需要在进程步骤中使用的Java类的名称。 您需要传递将包含注释的元数据属性名称。 在下面的屏幕快照中， managerComments是将存储注释的元数据属性。

![workflowcomments1](assets/workflowcomments1.gif)

要在系统上测试此功能，请执行以下步骤：
* [确保将工作流中的进程步骤配置为使用捕获工作流注释](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developingwithserviceuser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 此捆绑包中包含用于捕获注释并将其存储为元数据属性的示例代码

* [将与本文相关的资源下载并解压缩到您的文件系统](assets/capturecomments.zip)这些资源包含工作流模型和示例自适应表单。

* 使用包管理器将2个zip文件导入AEM

* [通过浏览到此URL预览表单](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填写表单字段并提交表单

* [检查您的AEM收件箱](http://localhost:4502/aem/inbox)

* 从收件箱中打开任务并提交表单。 请在出现提示时输入一些备注。

这些注释存储在AEM存储库中名为`managerComments`的元数据属性中。 要检查注释，请以管理员身份登录crx。 工作流实例存储在以下路径中：

`/var/workflow/instances/server0`

选择适当的工作流实例，并检查元数据节点中的属性managerComments。
