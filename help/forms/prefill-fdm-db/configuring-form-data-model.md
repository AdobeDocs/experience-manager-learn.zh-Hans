---
title: 配置表单数据模型
description: 基于RDBMS数据源创建表单数据模型
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 1%

---



# 配置表单数据模型

## Apache Sling Connection Pooled DataSource

创建RDBMS支持的表单数据模型的第一步是配置Apache Sling Connection Pooled DataSource。 要配置数据源，请按照以下列出的步骤操作：

* 将浏览器指向[configMgr](http://localhost:4502/system/console/configMgr)
* 搜索&#x200B;**Apache Sling Connection Pooled DataSource**
* 添加新条目并提供如屏幕截图所示的值。
* ![数据源](assets/data-source.png)
* 保存更改

>[!NOTE]
>JDBC连接URI、用户名和密码将根据MySQL数据库配置而更改。


## 创建表单数据模型

* 将浏览器指向[数据集成](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* 单击&#x200B;_创建_->_表单数据模型_
* 为表单数据模型（如&#x200B;**Employee**）提供有意义的名称和标题
* 单击&#x200B;_下一步_
* 选择在前面部分（论坛）中创建的数据源
* 单击&#x200B;_创建_->编辑以在编辑模式下打开新创建的表单数据模型
* 展开&#x200B;_forums_&#x200B;节点，查看员工模式。 展开employee节点以查看2个表

## 将图元添加到模型

* 确保employee节点已展开
* 选择新实体和受益人实体，然后单击&#x200B;_添加选定项_

## 将读取服务添加到新实体

* 选择新实体
* 单击&#x200B;_编辑属性_
* 从“读取服务”下拉列表中选择“获取”
* 单击+图标以向获取服务添加参数
* 指定屏幕截图中显示的值
* ![get-service](assets/get-service.png)
>[!NOTE]
> get服务需要映射到新实体的empID列的值。有多种方法可传递此值，在本教程中，empID将传递名为empID的请求参数。
* 单击&#x200B;_完成_&#x200B;以保存get服务的参数
* 单击&#x200B;_完成_&#x200B;以保存对表单数据模型所做的更改

## 添加2个实体之间的关联

在数据库实体之间定义的关联不会在表单数据模型中自动创建。 需要使用表单数据模型编辑器定义实体之间的关联。 每个新实体都可以有一个或多个受益人，我们需要定义新实体和受益实体之间的一对多关联。
以下步骤将引导您完成创建一对多关联的过程

* 选择新实体并单击&#x200B;_添加关联_
* 为关联和其他属性提供有意义的标题和标识符，如以下屏幕截图所示
   ![关联](assets/association-entities-1.png)

* 单击“参数”部分下的&#x200B;_edit_&#x200B;图标

* 指定此屏幕快照中所示的值
* ![association-2](assets/association-entities.png)
* **我们使用受益人和新实体的empID列链接这两个实体。**
* 单击&#x200B;_完成_&#x200B;以保存更改

## 测试表单数据模型

我们的表单数据模型现在有&#x200B;**_get_**&#x200B;服务，该服务接受empID并返回新客户及其受益人的详细信息。 要测试获取服务，请按照以下列出的步骤操作。

* 选择新实体
* 单击&#x200B;_测试模型对象_
* 提供有效的empID并单击&#x200B;_Test_
* 您应当获得如下屏幕截图所示的结果
* ![test-fdm](assets/test-form-data-model.png)
