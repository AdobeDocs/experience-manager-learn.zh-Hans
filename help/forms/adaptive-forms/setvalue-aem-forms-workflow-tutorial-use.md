---
title: 在AEM Forms工作流程中使用setvalue
description: 在AEM Forms OSGI中设置自适应Forms中提交数据的元素值
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 105
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# 在AEM Forms工作流程中使用setvalue

在AEM Forms OSGI工作流中的自适应Forms提交数据设置XML元素的值。

![设置值](assets/setvalue.png)

用于具有设置值组件的LiveCycle，该组件允许您设置XML元素的值。

根据此值，使用XML填充表单时，您可以隐藏/禁用表单的某些字段或面板。

在AEM Forms OSGi中 — 我们必须编写自定义OSGi捆绑包才能在XML中设置值。 该捆绑包作为本教程的一部分提供。
我们使用AEM Workflow中的“流程步骤”。 我们将“Set Value of Element in XML”OSGi捆绑包与此流程步骤关联。
我们需要将两个参数传递给设置值捆绑包。 第一个参数是需要设置其值的XML元素的XPath。 第二个参数是需要设置的值。
例如，在上面的屏幕截图中，我们将intialstep元素的值设置为“N”。
根据此值，可隐藏或显示自适应Forms中的某些面板。
在我们的示例中，我们提供了一个简单的休息时间申请表。 此表单的发起人填写其姓名和休息日期。 提交后，此表单将转至“管理员”进行审核。 当管理员打开表单时，第一个面板上的字段被禁用。 这是因为我们已经将XML中初始步骤元素的值设置为“N”。

根据初始步骤字段值，我们显示第二个面板，“管理员”可以批准或拒绝请求

请使用规则编辑器查看针对“请求休息时间”字段设置的规则。

要在本地系统上部署资产，请执行以下步骤：

* [部署Developingwithserviceuser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署示例捆绑包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 这是自定义OSGI捆绑包，允许您在提交的xml数据中设置元素的值

* [下载并解压缩zip文件的内容](assets/setvalueassets.zip)
* 将浏览器指向 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入并安装setValueWorkflow.zip。 这里有示例工作流模型。
* 将浏览器指向 [Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 单击创建 | 文件上传
* 上传TimeOfRequestForm.zip
* 打开 [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填写3个必填字段并提交
* 以“管理员”身份登录到AEM（如果尚未登录）
* 转到 [&quot;AEM收件箱&quot;](http://localhost:4502/aem/inbox)
* 打开“审阅休息时间请求”表单
* 请注意，第一个面板中的字段已禁用。 这是因为该表单正由审阅人打开。 此外，请注意批准或拒绝请求的面板现在可见

>[!NOTE]
>
>您可以通过启用以下项的记录器来启用调试日志记录
>com.aemforms.setvalue.core.SetValueinXml
>将浏览器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>确保自适应表单提交选项中的数据文件路径设置为“Data.xml”。 这是因为流程步骤在有效负荷文件夹下查找名为Data.xml的文件
