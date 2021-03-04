---
title: AEM Forms with Marketo（第3部分）
seo-title: AEM Forms with Marketo（第3部分）
description: 使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
seo-description: 使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
feature: 自适应Forms，表单数据模型
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# 配置数据源

AEM Forms数据集成允许您配置和连接到不同的数据源。 现成支持以下类型。 但是，只需进行少量自定义，您也可以与其他数据源集成。

1. 关系数据库 — MySQL、Microsoft SQL Server、IBM DB2和Oracle RDBMS。
1. AEM用户用户档案
1. REST风格的Web服务
1. 基于SOAP的Web服务
1. OData服务

为了将AEM Forms与Marketo集成，我们将使用REST风格的Web服务。 集成的第一步是配置[数据源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 请使用本教程中提供的Swagger文件。以下屏幕截图显示配置数据源时需要指定的重要属性。
![data](assets/datasource.jfif)

“marketo.json”是swagger文件，作为本教程资源的一部分提供给您。
属性主机特定于您的Marketo实例。
身份验证类型是自定义的，身份验证实施必须与“AemForms With Marketo”匹配。 （除非您在代码中更改了此项）。

## 创建表单数据模型

之后，配置数据源下一步是创建基于上一步中配置的数据源的表单数据模型。 要创建表单数据模型，请执行以下步骤：

将浏览器指向[数据集成页面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 这将列表在AEM实例上创建的所有数据集成。

1. 单击创建 |表单数据模型
1. 提供有意义的标题（如FormsAndMarketo），然后单击“下一步”
1. 选择在前一步骤中配置的数据源，然后单击创建并编辑以在编辑模式下打开表单数据模型
1. 展开“FormsAndMarketo”节点。 展开“服务”节点
1. 选择第一个“获取”操作
1. 单击“添加选定项”
1. 单击“添加关联的模型对象”(Add Associated Model Objects)对话框中的“全选”(Select All)，然后单击“添加”(Add)
1. 通过单击“保存”按钮保存表单数据模型
1. 选项卡
1. 选择列出的唯一服务，然后单击“Test Service”
1. 提供有效的leadId并单击“测试”。 如果一切顺利，您应当返回潜在客户详细信息，如以下屏幕截图所示
   ![测试结果](assets/testresults.jfif)
