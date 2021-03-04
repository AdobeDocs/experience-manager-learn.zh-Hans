---
title: 在AEM Forms工作流中设置Json数据元素的值
seo-title: 在AEM Forms工作流中设置Json数据元素的值
description: 随着自适应表单被路由到AEM工作流中的不同用户，系统会要求根据查看表单的人员隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常设置隐藏字段的值。 可以根据此隐藏字段的值业务规则进行创作，以隐藏/禁用相应的面板或字段。
seo-description: 随着自适应表单被路由到AEM工作流中的不同用户，系统会要求根据查看表单的人员隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常设置隐藏字段的值。 可以根据此隐藏字段的值业务规则进行创作，以隐藏/禁用相应的面板或字段。
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: 自适应表单，工作流
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 1%

---


# 在AEM Forms工作流{#setting-value-of-json-data-element-in-aem-forms-workflow}中设置JSON数据元素的值

随着自适应表单被路由到AEM工作流中的不同用户，系统会要求根据查看表单的人员隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常设置隐藏字段的值。 可以根据此隐藏字段的值业务规则进行创作，以隐藏/禁用相应的面板或字段。

![设置json数据中元素的值](assets/capture-3.gif)

在AEM Forms OSGI — 中，我们必须编写自定义OSGi捆绑包来设置JSON数据元素的值。 本教程提供了该包。

我们在AEM工作流中使用流程步骤。 我们将“在Json中设置元素值”OSGi捆绑与此过程步骤关联。

我们需要将两个参数传递给设置值包。 第一个参数是需要设置其值的元素的路径。 第二个参数是需要设置的值。

例如，在上面的屏幕截图中，我们将intialStep元素的值设置为“N”

afData.afUnboundData.data.initialStep,N

在我们的示例中，我们有一个简单的结束请求表。 此表单的发起人会填写其姓名和结束日期。 提交后，此表单将转到“管理者”进行审阅。 当管理器打开表单时，将禁用第一个面板上的字段。 这是因为我们已将JSON数据中初始步骤元素的值设置为N。

根据初始步骤字段值，我们会显示“管理者”可批准或拒绝请求的审批者面板。

请查看针对“初始步骤”设置的规则。 根据initialStep字段的值，我们使用表单数据模型获取用户详细信息，并填充相应的字段并隐藏/禁用相应的面板。

要在本地系统上部署资源，请执行以下操作：

* [下载和部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是自定义OSGI捆绑包，它允许您在提交的json数据中设置元素的值。

* [下载并解压zip文件的内容](assets/set-value-jsondata.zip)
   * 将浏览器指向[包管理器](http://localhost:4502/crx/packmgr/index.jsp)
      * 导入并安装SetValueOfElementInJSONDataWorkflow.zip。此包具有与表单关联的示例工作流模型和表单数据模型。

* 将浏览器指向[Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 |文件上传
* 上传TimeOffRequestForm.zip文件
   **此表单是使用AEM Forms 6.4构建的。请确保您使用的是AEM Forms 6.4或更高版本**
* 打开[表单](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填写开始和结束日期并提交表单。
* 转到[&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* 打开与任务关联的表单。
* 请注意，第一个面板中的字段处于禁用状态。
* 请注意，现在可以看到批准或拒绝请求的面板。

>[!NOTE]
>
>由于我们使用用户用户档案预填充自适应表单，请确保管理员[用户用户档案信息](http://localhost:4502/security/users.html)。 请至少确保已设置FirstName、LastName和“电子邮件”字段值。
>可通过从此处](http://localhost:4502/system/console/slinglog)为com.aemforms.setvalue.core.SetValueInJson [启用记录器来启用调试日志记录

>[!NOTE]
>
>用于设置JSON数据中数据元素值的OSGi捆绑当前支持一次设置一个元素值的功能。 如果要设置多个元素值，则需要多次使用流程步骤。
>
>确保自适应表单的提交选项中的数据文件路径设置为“Data.xml”。 这是因为进程步骤中的代码在有效负荷文件夹下查找名为Data.xml的文件。
