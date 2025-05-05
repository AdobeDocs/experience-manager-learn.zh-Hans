---
title: 创建自适应表单
description: 创建和配置自适应表单以使用表单数据模型的预填充服务
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
duration: 124
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 3%

---

# 创建自适应表单

到目前为止，我们已经创建了

* 具有2个表的数据库 — `newhire`和`beneficiaries`
* 已配置Apache Sling连接池化数据源
* 基于RDBMS的表单数据模型

下一步是创建和配置自适应表单以使用表单数据模型。  若要开始，您可以[下载并导入](assets/fdm-demo-af.zip)示例表单。 此示例表单中有一个部分用于显示员工详细信息，另一个部分用于列出员工的受益人。

## 将表单与表单数据模型关联

本课程随附的示例表单未关联任何表单数据模型。 要将表单配置为使用表单数据模型，我们需要执行以下操作：

* 选择FDMDemo表单
* 单击&#x200B;_属性_->_表单模型_
* 从下拉列表中选择表单数据模型
* 搜索并选择您在前面的课程中创建的表单数据模型。
* 单击&#x200B;_保存并关闭_

## 配置预填充服务

第一步是关联表单的预填充服务。 要关联预填充服务，请按照以下所述步骤操作

* 选择`FDMDemo`表单
* 单击&#x200B;_编辑_&#x200B;以在编辑模式下打开表单
* 在内容层次结构中选择表单容器，然后单击扳手图标以打开其属性工作表
* 从预填充服务下拉列表中选择&#x200B;_表单数据模型预填充服务_
* 单击蓝☑以保存更改

* ![预填充服务](assets/fdm-prefill.png)

## 配置员工详细信息

下一步是将自适应表单的文本字段绑定到表单数据模型元素。 您必须打开以下字段的属性表并设置其bindRef，如下所示


| 字段名 | 绑定引用 |
|------------|--------------------|
| 名字 | /newhire/名字 |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>您可以随意添加其他文本字段，并将其绑定到适当的表单数据模型元素

## 配置受益人表

下一步是以表格形式显示员工的受益人。 提供的示例表单有一个表，其中有4列和单行。 我们需要根据受益人数量来配置表格。

* 在编辑模式下打开该表单。
* 展开根面板 — >您的受益人 — >表
* 选择Row1并单击扳手图标以打开其属性表。
* 将Bind引用设置为&#x200B;**/newhire/GetEmployeeRelifinears**
* 将“Repeat Settings - Minimum Count（重复设置 — 最小计数）”设置为1， Maximum Count（最大计数）设置为5。
* 您的Row1配置应当与下面的屏幕快照类似
  ![行配置](assets/configure-row.PNG)
* 单击蓝☑以保存更改

## 绑定行单元格

最后，我们需要将行单元格绑定到表单数据模型元素。

* 展开根面板 — >您的受益人 — >表 — >行1
* 按照下表设置每个行单元格的绑定引用

| 行单元格 | Bind 引用 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeRefoldificators/firstname |
| 姓氏 | /newhire/GetEmployeeRefitures/lastname |
| 关系 | /newhire/GetEmployeeRefliants/relation |
| 百分比 | /newhire/GetEmployeeRefitures/percentage |

* 单击蓝☑以保存更改

## 测试您的表单

现在，我们需要在URL中使用适当的empID打开表单。 以下2个链接将使用数据库中的信息填充表单
empID=207[&#128279;](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)的表单
[empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)的表单

## 疑难解答

我的表单为空白，没有任何数据

* 确保表单数据模型返回正确结果。
* 表单与正确的表单数据模型相关联
* 检查字段绑定
* 查看stdout日志文件。 您应该会看到正在写入文件的empID。如果没有看到此值，则表示您的表单可能未使用提供的自定义模板。

未填充表

* 检查Row1绑定
* 确保正确设置Row1的重复设置（最小值= 1且最大值= 5或更大）
