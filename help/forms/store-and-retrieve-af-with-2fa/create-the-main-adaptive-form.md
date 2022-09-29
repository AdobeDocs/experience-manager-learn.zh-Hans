---
title: 创建主自适应表单
description: 创建自适应表单以捕获申请人信息，创建自适应表单以检索保存的自适应表单
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 创建主自适应表单

表单 **StoreAFWithAttachments** 是主要的自适应表单。 此自适应表单是用例的入口点。 在此表单中，会捕获用户详细信息（包括移动号码）。 此表单还能添加一些附件。 单击“保存并退出”按钮时，执行服务器端代码以将表单数据存储到数据库中，并生成唯一的应用程序ID并将其呈现给用户以进行安全保存。 此应用程序ID用于检索与应用程序关联的移动号码。

![主要应用程序表单](assets/6552.JPG)

此表单与 **bootboxjs540,storeAFWithAttachments** 之前在课程中创建的客户端库，以及在表单提交时触发的AEM工作流。


* 示例表单基于 [自定义自适应表单模板](assets/custom-template-with-page-component.zip) 需要导入到AEM中才能正确呈现示例表单。

* 已完成 [StoreAfWithAttachments表单](assets/store-af-with-attachments-form.zip) 可下载并导入到AEM实例中。

* 的 [与此表单关联的AEM工作流](assets/workflow-model-store-af-with-attachments.zip) 需要将导入到AEM实例中，才能使表单正常工作。
