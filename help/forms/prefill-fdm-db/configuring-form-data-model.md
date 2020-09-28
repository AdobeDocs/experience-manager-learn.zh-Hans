---
title: 配置表单数据模型
description: 基于RDBMS数据源创建表单数据模型
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 1%

---



# 配置表单数据模型

## Apache Sling Connection池化数据源

创建RDBMS支持的表单数据模型的第一步是配置Apache Sling Connection池化数据源。 要配置数据源，请按照下面列出的步骤操作：

* 将浏览器指向 [configMgr](http://localhost:4502/system/console/configMgr)
* 搜索Apache **Sling Connection池化数据源**
* 添加新条目并提供屏幕截图中显示的值。
* ![数据源](assets/data-source.png)
* 保存更改

>[!NOTE]
>JDBC连接URI、用户名和密码将根据MySQL数据库配置而发生更改。


## 创建表单数据模型

* 将浏览器指向数 [据集成](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Click _Create_->_Form Data Model_
* 为表单数据模型（如员工）提供有意义的名称和 **标题**
* Click _Next_
* 选择在前面部分（论坛）中创建的数据源
* 单击 _创建_->编辑以在编辑模式下打开新创建的表单数据模型
* 展开 _论坛_ 节点，查看员工模式。 展开employee节点以查看2个表

## 将图元添加到模型

* 确保员工节点已展开
* 选择新实体和受益人实体，然后单击“添 _加选定”_

## 向新实体添加读取服务

* 选择新实体
* 单击“编 _辑属性”_
* 从读取服务下拉列表中选择获取
* 单击+图标以向获取服务添加参数
* 指定屏幕截图中显示的值
* ![get-service](assets/get-service.png)
>[!NOTE]
> get服务需要映射到新实体的empID列的值。有多种方法可传递此值，在本教程中，empID将通过名为empID的请求参数传递。
* 单击 _完成_ ，以保存get服务的参数
* 单击 _完成_ ，保存对表单数据模型所做的更改

## 添加2个实体之间的关联

在数据库实体之间定义的关联不会在表单数据模型中自动创建。 需要使用表单数据模型编辑器定义实体之间的关联。 每个新实体都可以有一个或多个受益人，我们需要定义新实体和受益实体之间的一对多关联。
以下步骤将引导您完成创建一对多关联的过程

* 选择新实体并单击“添 _加关联”_
* 为关联和其他属性提供有意义的标题和标识符，如下面的屏幕截图所示
   ![协会](assets/association-entities-1.png)

* 单击“参 _数_ ”部分下的编辑图标

* 指定此屏幕快照中所示的值
* ![association-2](assets/association-entities.png)
* **我们使用受益人和新实体的empID列链接两个实体。**
* Click on _Done_ to save your changes

## 测试表单数据模型

我们的表单数据模型现 **_在已获_** 得接受empID并返回新客户及其受益人的详细信息的服务。 要测试获取服务，请按照下面列出的步骤操作。

* 选择新实体
* 单击“测 _试模型对象”_
* 提供有效的empID并单击“测 _试”_
* 您应当获得如下屏幕截图所示的结果
* ![test-fdm](assets/test-form-data-model.png)
