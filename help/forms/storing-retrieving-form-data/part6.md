---
title: 从MySQL数据库存储和检索表单数据
description: 多部分教程，指导您完成存储和检索表单数据时涉及的步骤
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 5%

---


# 在您的服务器上部署此组件

>[!NOTE]
>
>要在您的系统上运行此功能，需要满足以下条件
>
>* AEM Forms（版本6.3或更高版本）
>* MySql数据库


要在AEM Forms实例上测试此功能，请执行以下步骤

* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)下载和部署[MySql驱动程序Jar](assets/mysqldriver.jar)文件
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)下载和部署[OSGi包](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)下载并安装包含客户端库、自适应表单模板和自定义页面组件](assets/store-and-fetch-af-with-data.zip)的[包
* 使用[FormsAndDocuments界面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入[示例自适应表单](assets/sample-adaptive-form.zip)

* 使用MySql Workbench导入[form-data-db.sql](assets/form-data-db.sql)。 这将在数据库中创建必要的架构和表，以便本教程正常工作。
* 登录到[configMgr。](http://localhost:4502/system/console/configMgr) 搜索“Apache Sling连接池化数据源”。使用以下属性创建一个名为&#x200B;**SaveAndContinue**&#x200B;的新Apache Sling连接池化数据源条目：

| 属性名称 | 值 |
------------------------|---------------------------------------
| 数据源名称 | SaveAndContinue |
| JDBC驱动程序类 | com.mysql.cj.jdbc.Driver |
| JDBC连接URI | jdbc:mysql://localhost:3306/aemformstutorial |


* 打开[自适应表单](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填写一些详细信息，然后单击“保存并稍后继续”按钮。
* 您应该返回一个GUID为的URL。
* 复制URL并将其粘贴到新的浏览器选项卡中。 **确保URL末尾没有空格。**
* 自适应表单应使用上一步中的数据填充。
