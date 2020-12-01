---
title: 创建主自适应表单
description: 创建自适应表单以捕获申请人信息，创建自适应表单以检索保存的自适应表单
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# 创建主自适应表单

表单&#x200B;**StoreAFWithAttachments**&#x200B;是主自适应表单。 此自适应表单是用例的入口点。 在此表单中，将捕获用户详细信息（包括移动号码）。 此表单还可以添加一些附件。 单击“保存并退出”按钮时，将执行服务器端代码以将表单数据存储在数据库中，并生成唯一的应用程序ID并呈现给用户以进行安全保存。 此应用程序ID将用于检索与应用程序关联的移动号码。

![主要应用程序表单](assets/6552.JPG)

此表单与课程前面创建的&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;客户端库以及在提交表单时触发的AEM工作流相关联。


* 示例表单基于[自定义自适应表单模板](assets/custom-template-with-page-component.zip)，需要将该模板导入AEM，以使示例表单正确呈现。

* 可以下载完成的[StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)并将其导入AEM实例。

* 需要将与此表单](assets/workflow-model-store-af-with-attachments.zip)关联的[AEM工作流导入AEM实例，以使表单正常工作。



