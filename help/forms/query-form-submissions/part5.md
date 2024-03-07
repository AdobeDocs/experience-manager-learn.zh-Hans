---
title: 在本地服务器上部署示例
description: 多部分教程，可指导您完成查询Azure门户中存储的表单提交时涉及的步骤
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 在本地服务器上部署示例

要使该用例在本地服务器上正常工作，请按照下面列出的步骤操作。假定您的AEM实例在localhost 4502端口上运行。

* [安装包](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) 使用包管理器。

* 使用OSGi configMgr提供Azure门户凭据
  ![azure-portal](assets/azure-portal-config.png)
确保存储URI以斜杠结尾，并且SAS令牌以斜杠开头？
* 导航到 [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* 编辑以下3个数据源的身份验证设置以匹配您的环境
  ![数据源](assets/fdm-data-sources.png)

* 预览和提交 [ContactUs表单](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [查询您的表单提交](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

