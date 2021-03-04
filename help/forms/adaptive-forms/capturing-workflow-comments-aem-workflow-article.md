---
title: 在自适应Forms Workflow中捕获工作流注释
seo-title: 在自适应Forms Workflow中捕获工作流注释
description: 在AEM Workflow中捕获工作流注释
seo-description: 在AEM Workflow中捕获工作流注释
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: 工作流
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# 在自适应Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}中捕获工作流注释

>[仅适用于AEM Forms 6.4。在AEM Forms 6.5中，请使用变量功能实现此用例]

一个常见请求是允许将任务审阅人输入的注释包含在电子邮件中。 在AEM Forms 6.4中，没有开箱即用的机制来捕获用户输入的注释并将这些注释包含在电子邮件中。

为满足此要求，提供了一个示例OSGi捆绑包，可用于捕获注释并将这些注释存储为工作流元数据属性。

以下屏幕截图显示了如何使用[AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)中的进程步骤捕获注释并将它们存储为元数据属性。 “捕获工作流注释”是需要在流程步骤中使用的java类的名称。 您需要传递包含注释的元数据属性名称。 在以下屏幕截图中，managerComments是用于存储注释的元数据属性。

![workflowcomments1](assets/workflowcomments1.gif)

要在系统上测试此功能，请执行以下步骤：
* [确保将工作流中的流程步骤配置为使用捕获工作流注释](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [使用服务用户包部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此包包含捕获注释并将其存储为元数据属性的示例代码

* [将与本文相关的资产下载并解压缩到您的文件系统](assets/capturecomments.zip) 中资产包含工作流模型和示例自适应表单。

* 使用包管理器将2个zip文件导入AEM

* [预览表单，方法是浏览到此URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填写表单字段并提交表单

* [检查您的AEM收件箱](http://localhost:4502/aem/inbox)

* 从收件箱中打开任务并提交表单。 请在出现提示时输入一些评论。

注释将存储在crx中名为managerComments的元数据属性中。 以管理员身份检查注释登录crx。 工作流实例存储在以下路径中

/var/workflow/instances/server0

选择相应的工作流实例，并在元数据节点中检查属性managerComments。

