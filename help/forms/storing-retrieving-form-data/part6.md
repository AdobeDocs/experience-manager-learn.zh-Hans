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
source-wordcount: '248'
ht-degree: 4%

---


# 在您的服务器上部署此组件

>[!NOTE]
要在您的系统上运行AEMForms（版本6.3或更高版本）MYSQL数据库上，需要以下代码

要在您的AEM Forms实例上测试此功能，请执行以下步骤

* 将教程资源下 [载并解压](assets/store-retrieve-form-data.zip) 到您的本地系统
* 使用Felix Web控制台部署和开始techmarketingdemos.jar和mysqldriver.jar [捆绑套件](http://localhost:4502/system/console/configMgr)
* 使用MYSQL Workbench导入aemformstution.sql。 这将在您的模式库中创建必要的和表，以使本教程正常工作。
* 使用AEM包管理器导入 [StoreAndRetrieve.zip。](http://localhost:4502/crx/packmgr/index.jsp) 此包包含自适应表单模板、页面组件客户端库以及示例自适应表单和数据源配置。
* 登录 [configMgr。](http://localhost:4502/system/console/configMgr) 搜索“Apache Sling Connection Pooled DataSource”。 打开与Aemformsturation关联的数据源条目，并输入特定于数据库实例的用户名和密码。
* 打开自 [适应表单](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填写一些详细信息，然后单击“保存并稍后继续”按钮
* 您应返回包含GUID的URL。
* 复制URL并将其粘贴到新的浏览器选项卡中。 **确保URL末尾没有空格**
* 自适应表单应填充上一步中的数据
