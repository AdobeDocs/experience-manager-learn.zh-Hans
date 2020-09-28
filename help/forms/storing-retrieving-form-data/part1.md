---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据所涉及的步骤
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 3%

---

# 配置数据源

AEM支持与外部数据库集成的方式有很多。 数据库集成最常见的标准做法之一是通过configMgr使用Apache Sling Connection池化DataSource配置属 [性](http://localhost:4502/system/console/configMgr)。
第一步是在AEM中下载并部署相 [应的My SQL驱动程](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 序。
然后设置Sling Connection池化DataSource属性。 这些属性特定于您的数据库。 以下屏幕截图显示了本教程使用的设置。 数据库模式是作为本教程资源的一部分提供给您的。
![数据源](assets/data-source.png)

数据库有一个名为formdata的表，其中有3列，如数据库下的屏幕截![图所示](assets/data-base-tables.PNG)
