---
title: AEM Forms与Marketo（第3部分）
description: 教程介绍如何使用AEM Forms表单数据模型将AEM Forms与Marketo集成。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 1%

---

# 配置数据源

AEM Forms数据集成允许您配置并连接到不同的数据源。 支持开箱即用的以下类型。 但是，只需少量自定义，您也可以与其他数据源集成。

1. 关系数据库 — MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS
1. AEM用户配置文件
1. RESTful Web服务
1. 基于SOAP的Web服务
1. OData服务

为了将AEM Forms与Marketo集成，我们使用的是RESTful Web服务。 集成的第一步是配置 [数据源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 请使用本教程中提供的swagger文件。 以下屏幕截图显示了配置数据源时需要指定的重要属性。
![数据源](assets/datasource.png)

“marketo.json”是swagger文件，作为本教程资产的一部分提供给您。
资产主机特定于您的Marketo实例。
身份验证类型是自定义的，身份验证实施必须匹配“AemForms与Marketo”。 （除非您在代码中更改了此设置）。

## 创建表单数据模型

之后，配置数据源的下一步是创建基于上一步中配置的数据源的表单数据模型。 要创建表单数据模型，请执行以下步骤：

将浏览器指向 [数据集成页面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 这会列出在您的AEM实例上创建的所有数据集成。

1. 单击创建 | 表单数据模型
1. 提供有意义的标题，例如FormsAndMarketo ，然后单击“下一步”
1. 选择在之前步骤中配置的数据源，然后单击创建和编辑，以在编辑模式下打开表单数据模型
1. 展开“FormsAndMarketo”节点。 展开服务节点
1. 选择第一个“获取”操作
1. 单击添加选定项
1. 单击“添加关联的模型对象”对话框中的“全选”，然后单击“添加”
1. 单击保存按钮以保存表单数据模型
1. “服务”选项卡的选项卡
1. 选择列出的唯一服务，然后单击测试服务
1. 提供有效的leadId并单击Test。 如果一切进展顺利，您应该重新获取潜在客户详细信息，如下面的屏幕快照所示
   ![testresults](assets/testresults.png)

## 后续步骤

[将所有这些组件放在一起进行测试](./part4.md)
