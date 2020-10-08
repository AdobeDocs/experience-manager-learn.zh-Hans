---
title: 创建自适应表单
description: 创建并配置自适应表单以使用表单数据模型的预填服务
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# 创建自适应表单

到目前为止，我们已经创建了以下

* 具有2个表的数据库- `newhire` 和 `beneficiaries`
* 已配置Apache Sling Connection池化数据源
* 基于RDBMS的表单数据模型

下一步是创建并配置自适应表单以使用表单数据模型。  要获取头部开始，您可 [以下载并导入](assets/fdm-demo-af.zip) 示例表单。 示例表单中有一个部分用于显示员工详细信息，另一个部分用于列表员工的受益人。

## 将表单与表单数据模型关联

本课程提供的示例表单与任何表单数据模型都没有关联。 要配置表单以使用表单数据模型，我们需要执行以下操作：

* 选择FDMDemo表单
* 单击“属 _性_”->“_表单模型”_
* 从下拉式列表中选择表单数据模型
* 搜索并选择在前一课中创建的表单数据模型。
* Click on _Save &amp; Close_

## 配置预填服务

第一步是关联表单的预填服务。 要关联预填服务，请按照以下步骤操作

* 选择表 `FDMDemo` 单
* 单击 _编辑_ ，以在编辑模式下打开表单
* 在内容层次结构中选择表单容器，然后单击扳手图标以打开其属性表
* 从“ _预填服务”下拉列表_ ，选择“表单数据模型预填服务”
* 单击蓝☑色保存更改

* ![预填服务](assets/fdm-prefill.png)

## 配置员工详细信息

下一步是将自适应表单的文本字段绑定到表单数据模型元素。 您必须打开以下字段的属性表并设置其bindRef，如下所示


| 字段名称 | 绑定参考 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>您可以随意添加其他文本字段并将其绑定到相应的表单数据模型元素

## 配置受益人表

下一步是以表格形式显示员工的受益人。 提供的示例表单有一个表格，表格有4列和单行。 我们需要配置表格，以根据受益人的数量增加。

* 在编辑模式下打开表单。
* 展开根面板->您的受益人->表
* 选择Row1并单击扳手图标以打开其属性表。
* 将绑定引用设 **置为/newhire/GetEmployee受益人**
* 将重复设置——最小计数设置为1，最大计数设置为5。
* 您的Row1配置应类似于下面的屏幕快照
   ![行配置](assets/configure-row.PNG)
* 单击蓝色☑以保存更改

## 绑定行单元格

最后，我们需要将行单元格绑定到表单数据模型元素。

* 展开根面板->您的受益人->表->行1
* 根据下表设置每个行单元格的绑定引用

| 行单元格 | Bind 引用 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeBenserts/firstname |
| 姓氏 | /newhire/GetEmployeeBenalients/lastname |
| 关系 | /newhire/GetEmployeeWenneriters/relation |
| 百分比 | /newhire/GetEmployeeWennerities/perceptage |

* 单击蓝色☑以保存更改

## 测试表单

现在，我们需要在url中打开具有相应empID的表单。 以下2个链接将用empID=207的数据库[表单中的信息填充](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)[表单，empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## 疑难解答

我的表单为空，没有任何数据

* 确保表单数据模型返回正确的结果。
* 表单与正确的表单数据模型相关联
* 检查字段绑定
* 检查stdout日志文件。 您应当看到empID被写入到文件中。如果看不到此值，则表单可能未使用提供的自定义模板。

未填充表

* 检查Row1绑定
* 确保正确设置行1的重复设置（最小值=1，最大值= 5或更多）

