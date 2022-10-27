---
title: 在AEM Forms工作流中使用setvalue
description: 在AEM Forms OSGi中设置自适应Forms提交数据中元素的值
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 在AEM Forms工作流中使用setvalue

在AEM Forms OSGi工作流中设置自适应Forms提交数据中XML元素的值。

![SetValue](assets/setvalue.png)

LiveCycle，用于具有设置值组件，该组件允许您设置XML元素的值。

根据此值，在使用XML填充表单时，可以隐藏/禁用表单的某些字段或面板。

在AEM Forms OSGi中 — 我们必须编写自定义OSGi包才能在XML中设置值。 此包将作为本教程的一部分提供。
我们使用AEM工作流中的流程步骤。 我们将“在XML中设置元素值”OSGi包与此流程步骤相关联。
我们需要将两个参数传递到设置值包。 第一个参数是需要设置其值的XML元素的XPath。 第二个参数是需要设置的值。
例如，在上面的屏幕截图中，我们将initialstep元素的值设置为“N”。
根据此值，自适应Forms中的某些面板会被隐藏或显示。
在本例中，我们有一个简单的结束请求表单。 此表单的发起者填写其姓名和结束日期。 提交后，此表单将转至“管理员”进行审核。 管理员打开表单时，第一个面板上的字段会被禁用。 这是因为我们已将XML中初始步骤元素的值设置为“N”。

根据初始步骤字段值，我们显示第二个面板，“管理员”可以在该面板中批准或拒绝请求

请使用规则编辑器查看针对“请求的结束时间”字段设置的规则。

要在本地系统上部署资产，请执行以下步骤：

* [部署Developmingwithserviceuser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署示例包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是自定义OSGI包，用于设置提交的xml数据中元素的值

* [下载并解压缩zip文件的内容](assets/setvalueassets.zip)
* 将您的浏览器指向 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入并安装setValueWorkflow.zip。 这里有工作流模型的示例。
* 将您的浏览器指向 [Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 |文件上传
* 上载TimeOfRequestForm.zip
* 打开 [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填写3个必填字段并提交
* 以“管理员”身份登录到AEM（如果尚未登录）
* 转到 [&quot;AEM收件箱&quot;](http://localhost:4502/aem/inbox)
* 打开“审核请求结束时间”窗体
* 请注意，第一个面板中的字段处于禁用状态。 这是因为表单由审阅人打开。 此外，请注意，现在可以看到批准或拒绝请求的面板

>[!NOTE]
>
>您可以通过为启用日志记录器来启用调试日志记录
>com.aemforms.setvalue.core.SetValueinXml
>将您的浏览器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>确保将自适应表单提交选项中的数据文件路径设置为“Data.xml”。 这是因为流程步骤在有效负载文件夹下查找名为Data.xml的文件
