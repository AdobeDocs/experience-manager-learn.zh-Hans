---
title: 测试欢迎套件解决方案
description: 部署解决方案资产以测试解决方案
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 31
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 部署和测试示例资产

[安装欢迎套件包](assets/welcomekit.zip). 此包中包含要包含在欢迎套件中的页面模板、用于列出资产的自定义组件、示例工作流、电子邮件模板和示例PDF文档。
[安装欢迎套件包](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). 此捆绑包中包含用于创建页面的代码和用于返回要在网页中显示的资产的Java类。
[安装自适应表单示例](assets/account-openeing-form.zip)
配置Day CQ邮件服务。 工作流将欢迎套件链接发送到需要正确配置SMTP的表单提交者。
根据要求配置工作流的发送电子邮件组件
[预览表单](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
输入您的详细信息，然后选择一个或多个共同基金并提交表单。您应会收到一封电子邮件，其中包含欢迎套件链接
