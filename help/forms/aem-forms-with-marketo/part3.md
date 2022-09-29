---
title: AEM Forms与Marketo（第3部分）
description: 有关使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# 配置数据源

AEM Forms数据集成允许您配置不同的数据源并将其连接到不同的数据源。 支持开箱即用地使用以下类型。 但是，通过少量自定义，您也可以与其他数据源集成。

1. 关系数据库 — MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS。
1. AEM用户配置文件
1. RESTful Web服务
1. 基于SOAP的Web服务
1. OData服务

对于AEM Forms与Marketo的集成，我们使用的是RESTful Web服务。 集成的第一步是配置 [数据源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 请使用本教程中提供的swagger文件。 以下屏幕截图显示了配置数据源时需要指定的重要属性。
![数据源](assets/datasource.jfif)

“marketo.json”是swagger文件，将作为本教程资产的一部分提供给您。
资产主机特定于您的Marketo实例。
身份验证类型是自定义类型，且身份验证实施必须与“AemForms与Marketo”匹配。 （除非您在代码中更改了此设置）。

## 创建表单数据模型

之后，配置数据源的下一步是创建一个表单数据模型，该模型基于前面步骤中配置的数据源。 要创建表单数据模型，请执行以下步骤：

将您的浏览器指向 [数据集成页面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 此列表列出了在您的AEM实例中创建的所有数据集成。

1. 单击创建 |表单数据模型
1. 提供有意义的标题（如FormsAndMarketo），然后单击“下一步”
1. 选择在前面的步骤中配置的数据源，然后单击创建和编辑以在编辑模式下打开表单数据模型
1. 展开“FormsAndMarketo”节点。 展开“服务”节点
1. 选择第一个“Get”操作
1. 单击添加选定项
1. 单击“添加关联的模型对象”(Add Associated Model Objects)对话框中的“全选”(Select All)，然后单击“添加”(Add)
1. 通过单击保存按钮保存表单数据模型
1. “服务”选项卡
1. 选择列出的唯一服务，然后单击“测试服务”
1. 提供有效的leadId并单击“测试”。 如果一切顺利，您应该返回潜在客户详细信息，如以下屏幕截图所示
   ![测试结果](assets/testresults.jfif)
