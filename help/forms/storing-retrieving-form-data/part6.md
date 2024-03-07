---
title: 存储和检索MySQL数据库中的表单数据 — 部署
description: 多部分教程将指导您完成存储和检索表单数据所涉及的步骤
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 67
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---

# 在您的服务器上部署此项

>[!NOTE]
>
>若要在系统上运行此设置，需要满足以下条件
>
>* AEM Forms（版本6.3或更高版本）
>* MySql数据库

要在您的AEM Forms实例上测试此功能，请执行以下步骤

* 下载并部署 [MySql驱动程序Jar](assets/mysqldriver.jar) 文件使用 [felix web控制台](http://localhost:4502/system/console/bundles)
* 下载并部署 [OSGi包](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) 使用 [felix web控制台](http://localhost:4502/system/console/bundles)
* 下载并安装 [包含客户端库、自适应表单模板和自定义页面组件的包](assets/store-and-fetch-af-with-data.zip) 使用 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 导入 [自适应表单示例](assets/sample-adaptive-form.zip) 使用 [FormsAndDocuments界面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 导入 [form-data-db.sql](assets/form-data-db.sql) 使用MySql Workbench。 这将在数据库中创建必要的方案和表，以供本教程使用。
* 登录 [configMgr。](http://localhost:4502/system/console/configMgr) 搜索“Apache Sling连接池化数据源”。 创建新的Apache Sling连接池数据源条目，名为 **SaveAndContinue** 使用以下属性：

| 属性名称 | 价值 |
| ------------------------|---------------------------------------|
| 数据源名称 | `SaveAndContinue` |
| JDBC驱动程序类 | `com.mysql.cj.jdbc.Driver` |
| JDBC连接uri | `jdbc:mysql://localhost:3306/aemformstutorial` |

* 打开 [自适应表单](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填写一些详细信息，然后单击“保存并稍后继续”按钮。
* 您应该获取其中包含GUID的URL。
* 复制URL并将其粘贴到新的浏览器选项卡中。 **确保URL末尾没有空格。**
* 自适应表单应使用上一步的数据进行填充。
