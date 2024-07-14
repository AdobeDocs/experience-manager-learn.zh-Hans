---
title: 创建主自适应表单
description: 创建自适应表单以捕获申请人信息和自适应表单以检索保存的自适应表单
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 创建主自适应表单

表单&#x200B;**StoreAFWithAttachments**&#x200B;是主要的自适应表单。 此自适应表单是用例的入口点。 在此表单中捕获了用户详细信息，包括手机号码。 此表单还可以添加一些附件。 当单击“保存并退出”按钮时，执行服务器端代码以将表单数据存储在数据库中，并且生成唯一的应用程序ID并将其呈现给用户以便安全保存。 此应用程序ID用于检索与应用程序关联的手机号码。

![主应用程序表单](assets/6552.JPG)

此表单与之前在课程中创建的&#x200B;**bootboxjs540、storeAFWithAttachments**&#x200B;客户端库相关联，并且与表单提交时触发的AEM工作流相关联。


* 示例表单基于[自定义自适应表单模板](assets/custom-template-with-page-component.zip)，需要将其导入到AEM中才能正确呈现示例表单。

* 可以下载已完成的[StoreAfWithAttachments表单](assets/store-af-with-attachments-form.zip)并将其导入到您的AEM实例中。

* 与此表单](assets/workflow-model-store-af-with-attachments.zip)关联的[AEM工作流需要导入到您的AEM实例中，该表单才能正常工作。


## 后续步骤

[创建表单检索已保存的表单](./retrieve-saved-form.md)