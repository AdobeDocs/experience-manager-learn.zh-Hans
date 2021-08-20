---
title: 创建自适应表单
description: 创建并配置自适应表单以使用表单数据模型的预填充服务
feature: 自适应表单
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: 开发
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# 创建自适应表单

到目前为止，我们已经创建了以下

* 具有2个表的数据库 — `newhire`和`beneficiaries`
* 已配置Apache Sling连接池化数据源
* 基于RDBMS的表单数据模型

下一步是创建并配置自适应表单以使用表单数据模型。  要抢先一步，您可以下载并导入](assets/fdm-demo-af.zip)示例表单。 [示例表单有一个部分用于显示员工详细信息，另一个部分用于列出员工的受益人。

## 将表单与表单数据模型关联

本课程提供的示例表单与任何表单数据模型都不相关。 要配置表单以使用表单数据模型，我们需要执行以下操作：

* 选择FDMDemo表单
* 单击&#x200B;_属性_->_表单模型_
* 从下拉列表中选择表单数据模型
* 搜索并选择在前面的课程中创建的表单数据模型。
* 单击&#x200B;_保存并关闭_

## 配置预填充服务

第一步是关联表单的预填充服务。 要关联预填充服务，请按照以下所述步骤操作

* 选择`FDMDemo`表单
* 单击&#x200B;_编辑_&#x200B;以在编辑模式下打开表单
* 在内容层次结构中选择“表单容器”，然后单击扳手图标以打开其属性表
* 从预填充服务下拉列表中选择&#x200B;_表单数据模型预填充服务_
* 单击蓝☑以保存更改

* ![预填充服务](assets/fdm-prefill.png)

## 配置员工详细信息

下一步是将自适应表单的文本字段绑定到表单数据模型元素。 您必须打开以下字段的属性表并设置其bindRef，如下所示


| 字段名称 | 绑定参考 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>请随时添加其他文本字段，并将它们绑定到相应的表单数据模型元素

## 配置受益人表

下一步是以表格形式显示员工的受益人。 提供的示例表单有一个表格，其中有4列和单行。 我们需要配置表以根据受益人的数量而增长。

* 在编辑模式下打开表单。
* 展开根面板 — >您的受益人 — >表
* 选择Row1并单击扳手图标以打开其属性表。
* 将绑定引用设置为&#x200B;**/newhire/GetEmployeeWeniters**
* 将重复设置 — 最小计数设置为1，最大计数设置为5。
* 您的Row1配置应类似于下面的屏幕快照
   ![row-configure](assets/configure-row.PNG)
* 单击蓝色☑以保存更改

## 绑定行单元格

最后，我们需要将行单元格绑定到表单数据模型元素。

* 展开根面板 — >您的受益人 — >表 — >Row1
* 根据下表设置每个行单元格的绑定引用

| 行单元格 | Bind 引用 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeWinneters/firstname |
| 姓氏 | /newhire/GetEmployeeWinenters/lastname |
| 关系 | /newhire/GetEmployeeWinneters/relation |
| 百分比 | /newhire/GetEmployeeWinneters/percentage |

* 单击蓝色☑以保存更改

## 测试表单

现在，我们需要在url中打开具有相应empID的表单。 以下2个链接将使用来自数据库的信息填充表单
[具有empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)的表单
[具有empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)的表单

## 疑难解答

我的表单为空，没有任何数据

* 确保表单数据模型返回正确的结果。
* 表单与正确的表单数据模型相关联
* 检查字段绑定
* 检查stdout日志文件。 您应会看到empID正在写入文件。如果您没有看到此值，则表单可能没有使用提供的自定义模板。

未填充表

* 检查Row1绑定
* 确保正确设置Row1的重复设置（最小=1，最大= 5或更多）

