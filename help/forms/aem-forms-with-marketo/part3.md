---
title: AEM Forms与市场（第3部分）
seo-title: AEM Forms与市场（第3部分）
description: 教程，将AEM Forms与Marketo结合使用AEM Forms表单数据模型。
seo-description: 教程，将AEM Forms与Marketo结合使用AEM Forms表单数据模型。
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# 配置数据源

AEM Forms数据集成允许您配置和连接不同的数据源。 现成支持以下类型。 但是，只需进行少量自定义，您也可以与其他数据源集成。

1. 关系数据库- MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS。
1. AEM用户用户档案
1. REST风格的Web服务
1. 基于SOAP的Web服务
1. OData服务

为了将AEM Forms与Marketo集成，我们将使用RESTful Web服务。 集成的第一步是配置[数据源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 请使用本教程中提供的Swagger文件。以下屏幕截图显示配置数据源时需要指定的重要属性。
![数据源](assets/datasource.jfif)

“marketo.json”是swagger文件，作为本教程资源的一部分提供给您。
属性主机特定于您的Marketo实例。
身份验证类型是自定义的，身份验证实施必须与“AemForms与Marketo匹配”。 （除非您在代码中更改了此项）。

## 创建表单数据模型

之后，配置数据源下一步是创建基于之前步骤中配置的数据源的表单数据模型。 要创建表单数据模型，请按照以下步骤操作：

将浏览器指向[数据集成页面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 这将列表在AEM实例上创建的所有数据集成。

1. 单击创建 |表单数据模型
1. 提供有意义的标题（如FormsAndMarketo），然后单击“下一步”
1. 选择之前步骤中配置的数据源，然后单击创建并编辑以在编辑模式下打开表单数据模型
1. 展开“FormsAndMarketo”节点。 展开“服务”节点
1. 选择第一个“获取”操作
1. 单击“添加选定项”
1. 单击“添加关联的模型对象”(Add Associated Model Objects)对话框中的“全选”(Select All)，然后单击“添加”(Add)
1. 通过单击保存按钮保存表单数据模型
1. 选项卡，指向“服务”选项卡
1. 选择列出的唯一服务，然后单击“测试服务”
1. 提供有效的leadId并单击“测试”。 如果一切顺利，您应当返回潜在客户详细信息，如以下屏幕截图所示
   ![测试结果](assets/testresults.jfif)
