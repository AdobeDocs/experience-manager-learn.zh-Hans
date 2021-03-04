---
title: 在AEM Forms工作流程中使用setvalue
seo-title: 在AEM Forms Workflow中使用setvalue
description: 在AEM Forms OSGI中设置自适应Forms提交数据中元素的值
seo-description: 在AEM Forms OSGI中设置自适应Forms提交数据中元素的值
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: 自适应表单，工作流
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---


# 在AEM Forms工作流程中使用setvalue

在AEM Forms OSGI工作流中设置Adaptive Forms提交数据中XML元素的值。

![SetValue](assets/setvalue.png)

LiveCycle用于具有设置值组件，该组件允许您设置XML元素的值。

根据此值，当表单填充有XML时，您可以隐藏/禁用表单的某些字段或面板。

在AEM Forms OSGI中，我们必须编写自定义OSGi捆绑包才能在XML中设置值。 本教程提供了该包。
我们在AEM工作流中使用流程步骤。 我们将“在XML中设置元素值”OSGi捆绑与此过程步骤关联。
我们需要将两个参数传递给设置值包。 第一个参数是需要设置其值的XML元素的XPath。 第二个参数是需要设置的值。
例如，在上面的屏幕截图中，我们将intialstep元素的值设置为“N”。
根据此值，自适应Forms中的某些面板会被隐藏或显示。
在我们的示例中，我们有一个简单的结束请求表。 此表单的发起人会填写其姓名和结束日期。 提交后，此表单将转到“admin”进行审阅。 管理员打开表单时，第一个面板上的字段将被禁用。 这是因为我们已将XML中初始步骤元素的值设置为“N”。

根据初始步骤字段值，我们显示第二个面板，“管理员”可在其中批准或拒绝请求

请使用规则编辑器查看针对“请求结束时间”字段设置的规则。

要在本地系统上部署资产，请按照以下步骤操作：

* [使用服务用户包部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署示例包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。这是自定义OSGI包，它允许您在提交的xml数据中设置元素的值

* [下载并解压zip文件的内容](assets/setvalueassets.zip)
* 将浏览器指向[包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入并安装setValueWorkflow.zip。 它包含工作流模型示例。
* 将浏览器指向[Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 |文件上传
* 上传TimeOfRequestForm.zip
* 打开[TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填写3个必填字段并提交
* 以“admin”身份登录AEM（如果尚未登录）
* 转到[&quot;AEM Inbox&quot;](http://localhost:4502/aem/inbox)
* 打开“请求的审核时间”表单
* 请注意，第一个面板中的字段处于禁用状态。 这是因为审阅者正在打开表单。 另外，请注意，现在可以看到批准或拒绝请求的面板

>[!NOTE]
>
>可以通过为
>com.aemforms.setvalue.core.SetValueinXml
>将您的浏览器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>确保自适应表单的提交选项中的数据文件路径设置为“Data.xml”。 这是因为进程步骤在有效负荷文件夹下查找名为Data.xml的文件
