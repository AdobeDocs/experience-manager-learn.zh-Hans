---
title: 创建自适应表单
description: 创建并配置自适应表单以使用表单数据模型的预填服务
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: 开发
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 2%

---


# 创建自适应表单

到目前为止，我们已创建了以下

* 包含2个表的数据库 — `newhire`和`beneficiaries`
* 已配置Apache Sling Connection池化DataSource
* 基于RDBMS的表单数据模型

下一步是创建并配置自适应表单以使用表单数据模型。  要获取头部开始，您可以[下载并导入](assets/fdm-demo-af.zip)示例表单。 示例表单中有一个部分用于显示员工详细信息，另一个部分用于列表员工的受益人。

## 将表单与表单数据模型关联

本课程提供的示例表单不与任何表单数据模型关联。 要将表单配置为使用表单数据模型，需要执行以下操作：

* 选择FDMDemo表单
* 单击&#x200B;_属性_->_表单模型_
* 从下拉列表中选择表单数据模型
* 搜索并选择在上一课中创建的表单数据模型。
* 单击&#x200B;_保存并关闭_

## 配置预填服务

第一步是关联表单的预填服务。 要关联预填服务，请遵循以下步骤

* 选择`FDMDemo`表单
* 单击&#x200B;_编辑_&#x200B;以在编辑模式下打开表单
* 在内容层次结构中选择表单容器，然后单击扳手图标以打开其属性表
* 从“预填服务”下拉列表中选择&#x200B;_表单数据模型预填服务_
* 单击蓝☑以保存更改

* ![预填服务](assets/fdm-prefill.png)

## 配置员工详细信息

下一步是将自适应表单的文本字段绑定到表单数据模型元素。 您必须打开以下字段的属性表并设置其bindRef，如下所示


| 字段名称 | 绑定参考 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>可随意添加其他文本字段并将它们绑定到相应的表单数据模型元素

## 配置受益人表

下一步是以表格形式显示员工的受益人。 提供的示例表单有一个表格，表格有4列和单行。 我们需要根据受益者的数量来配置表格。

* 在编辑模式下打开表单。
* 展开根面板 — >您的受益人 — >表
* 选择Row1并单击扳手图标以打开其属性表。
* 将“绑定引用”设置为&#x200B;**/newhire/GetEmployeeWennieters**
* 将重复设置 — 最小计数设置为1，将最大计数设置为5。
* 您的Row1配置应类似于下面的屏幕快照
   ![行配置](assets/configure-row.PNG)
* 单击蓝色☑以保存更改

## 绑定行单元格

最后，我们需要将Row单元格绑定到Form Data Model元素。

* 展开根面板 — >您的受益人 — >表 — >行1
* 根据下表设置每个行单元格的绑定引用

| 行单元格 | Bind 引用 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployee受益人/名 |
| 姓氏 | /newhire/GetEmployeeWenielts/lastname |
| 关系 | /newhire/GetEmployeeWenilements/relation |
| 百分比 | /newhire/GetEmployeeWenilements/percentage |

* 单击蓝色☑以保存更改

## 测试表单

现在，我们需要在url中打开包含相应empID的表单。 以下2个链接将用数据库中的信息填充表单
[表单，empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)的表单

## 疑难解答

我的表单为空，没有任何数据

* 确保表单数据模型返回正确的结果。
* 表单与正确的表单数据模型关联
* 检查字段绑定
* 检查stdout日志文件。 您应当看到empID正被写入文件。如果看不到此值，则表单可能没有使用提供的自定义模板。

未填充表

* 检查Row1绑定
* 确保正确设置行1的重复设置（最小=1，最大=5或更大）

