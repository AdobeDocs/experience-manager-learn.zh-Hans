---
title: 在AEM Forms工作流中设置Json数据元素的值
description: 在AEM工作流中将自适应表单路由到不同的用户时，需要根据审核表单的人员来隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常会设置一个隐藏字段值。 可以根据此隐藏字段的值业务规则进行创作，以隐藏/禁用相应的面板或字段。
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---

# 在AEM Forms工作流中设置JSON数据元素的值 {#setting-value-of-json-data-element-in-aem-forms-workflow}

在AEM工作流中将自适应表单路由到不同的用户时，需要根据审核表单的人员来隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常会设置一个隐藏字段值。 可以根据此隐藏字段的值业务规则进行创作，以隐藏/禁用相应的面板或字段。

![在json数据中设置元素的值](assets/capture-3.gif)

在AEM Forms OSGi中 — 我们必须创建自定义OSGi包，以设置JSON数据元素的值。 此包将作为本教程的一部分提供。

我们使用AEM工作流中的流程步骤。 我们将“在Json中设置元素值”OSGi包与此流程步骤相关联。

我们需要将两个参数传递到设置值包。 第一个参数是需要设置其值的元素的路径。 第二个参数是需要设置的值。

例如，在上面的屏幕截图中，我们将intialStep元素的值设置为“N”

afData.afUnboundData.data.initialStep,N

在本例中，我们有一个简单的结束请求表单。 此表单的发起者填写其姓名和结束日期。 提交后，此表单将转至“经理”进行审核。 管理器打开表单时，第一个面板上的字段会被禁用。 这是因为我们已将JSON数据中初始步骤元素的值设置为N。

根据初始步骤字段值，我们会显示“审批者”面板，“经理”可在该面板中批准或拒绝请求。

请查看针对“初始步骤”设置的规则。 根据initialStep字段的值，我们使用表单数据模型获取用户详细信息，并填充相应的字段并隐藏/禁用相应的面板。

要在本地系统上部署资产，请执行以下操作：

* [下载和部署DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载和部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是自定义OSGI包，允许您在提交的json数据中设置元素的值。

* [下载并解压缩zip文件的内容](assets/set-value-jsondata.zip)
   * 将您的浏览器指向 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
      * 导入并安装SetValueOfElementInJSONDataWorkflow.zip。此包具有与表单关联的示例工作流模型和表单数据模型。

* 将您的浏览器指向 [Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 |文件上传
* 上传TimeOffRequestForm.zip文件
   **此表单是使用AEM Forms 6.4构建的。请确保您使用的是AEM Forms 6.4或更高版本**
* 打开 [表单](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填写开始和结束日期并提交表单。
* 转到 [&quot;收件箱&quot;](http://localhost:4502/aem/inbox)
* 打开与任务关联的表单。
* 请注意，第一个面板中的字段处于禁用状态。
* 请注意，现在显示了批准或拒绝请求的面板。

>[!NOTE]
>
>由于我们使用用户配置文件预填充自适应表单，因此请确保管理员 [用户配置文件信息 ](http://localhost:4502/security/users.html). 请至少确保已设置FirstName、LastName和Email字段值。
>您可以启用com.aemforms.setvalue.core.SetValueInJson的日志记录器，以启用调试日志记录 [从此处](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>当前，用于设置JSON数据中数据元素值的OSGi包支持一次设置一个元素值的功能。 如果要设置多个元素值，则需要多次使用流程步骤。
>
>确保将自适应表单提交选项中的数据文件路径设置为“Data.xml”。 这是因为流程步骤中的代码在有效负荷文件夹下查找名为Data.xml的文件。
