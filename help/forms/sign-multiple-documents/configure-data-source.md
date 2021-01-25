---
title: 配置AEM数据源
description: 配置MySQL支持的数据源以存储和检索表单数据
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 4%

---

# 配置数据源

AEM支持与外部数据库集成的方式有很多。 集成数据库的最常见方法之一是通过[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection池化DataSource配置属性。
第一步是下载并部署AEM中相应的[MySql驱动程序](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
创建Apache Sling Connection Pooled DataSource并提供以下屏幕快照中指定的属性。 数据库模式是作为本教程资源的一部分提供给您的。

![数据源](assets/data-source.PNG)

数据库有一个名为formdata的表，其中有3列，如下面的屏幕截图所示。

![数据库](assets/data-base.PNG)


>[!NOTE]
>请确保您命名数据源&#x200B;**aemformstutional**。 示例代码使用名称连接到数据库。

| 属性名称 | 值 |
------------------------|---------------------------------------
| 数据源名称 | 保存并继续 |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstutorial |

## 资产

创建模式的sql文件可从此处](assets/sign-multiple-forms.sql)下载。 [您需要使用MySql工作台导入此文件以创建模式和表。


