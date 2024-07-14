---
title: 在AEM Forms Workflow中设置Json数据元素的值
description: 由于自适应表单在AEM Workflow中被路由到不同的用户，因此需要根据查看表单的人来隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常设置隐藏字段的值。 可以基于此隐藏字段的值创作业务规则以隐藏/禁用相应的面板或字段。
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---

# 在AEM Forms Workflow中设置JSON数据元素的值 {#setting-value-of-json-data-element-in-aem-forms-workflow}

由于自适应表单在AEM Workflow中被路由到不同的用户，因此需要根据查看表单的人来隐藏或禁用某些字段或面板。 为了满足这些用例，我们通常设置隐藏字段的值。 可以基于此隐藏字段的值创作业务规则以隐藏/禁用相应的面板或字段。

![在JSON数据中设置元素的值](assets/capture-3.gif)

在AEM Forms OSGi中 — 我们必须创建一个自定义OSGi捆绑包以设置JSON数据元素的值。 该捆绑包作为本教程的一部分提供。

我们使用AEM Workflow中的“流程步骤”。 我们将“在Json中设置元素的值”OSGi捆绑包与此流程步骤关联。

我们需要将两个参数传递给设置值捆绑包。 第一个参数是需要设置其值的元素的路径。 第二个参数是需要设置的值。

例如，在上面的屏幕截图中，我们将initialStep元素的值设置为“N”

afData.afUnboundData.data.initialStep,N

在我们的示例中，我们提供了一个简单的休息时间申请表。 此表单的发起人填写其姓名和休息日期。 提交后，此表单将转至“经理”进行审核。 当管理器打开表单时，第一个面板上的字段被禁用。 这是因为我们已在JSON数据中将initial step元素的值设置为N。

根据初始步骤字段值，我们显示“审批者”面板，“经理”可以批准或拒绝请求。

请查看针对“初始步骤”设置的规则。 我们根据initialStep字段的值，使用表单数据模型提取用户详细信息，并填充相应的字段和隐藏/禁用相应的面板。

要在本地系统上部署资产，请执行以下操作：

* [下载并部署DevelopingWidthServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 这是自定义OSGI捆绑包，允许您在提交的json数据中设置元素的值。

* [下载并解压缩zip文件的内容](assets/set-value-jsondata.zip)
   * 将浏览器指向[包管理器](http://localhost:4502/crx/packmgr/index.jsp)
      * 导入并安装SetValueOfElementInJSONDataWorkflow.zip。此包中包含与表单关联的示例工作流模型和表单数据模型。

* 将浏览器指向[Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 | 文件上传
* 上传TimeOffRequestForm.zip文件
  **此表单是使用AEM Forms 6.4生成的。请确保您使用的是AEM Forms 6.4或更高版本**
* 打开[表单](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 填写开始日期和结束日期并提交表单。
* 转到[“收件箱”](http://localhost:4502/aem/inbox)
* 打开与任务关联的表单。
* 请注意，第一个面板中的字段已禁用。
* 请注意，用于批准或拒绝请求的面板现在可见。

>[!NOTE]
>
>由于我们使用用户配置文件预填充自适应表单，因此请确保管理员[用户配置文件信息](http://localhost:4502/security/users.html)。 至少要确保您已设置FirstName、LastName和Email字段值。
>您可以从此处](http://localhost:4502/system/console/slinglog)启用com.aemforms.setvalue.core.SetValueInJson [的记录器以启用调试日志记录

>[!NOTE]
>
>用于在JSON数据中设置数据元素值的OSGi包当前支持一次设置一个元素值的功能。 如果要设置多个元素值，则需要多次使用流程步骤。
>
>确保自适应表单提交选项中的数据文件路径设置为“Data.xml”。 这是因为流程步骤中的代码将在有效负荷文件夹下查找名为Data.xml的文件。
