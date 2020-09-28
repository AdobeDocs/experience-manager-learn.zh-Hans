---
title: 建立Web投放渠道文档
seo-title: 建立Web投放渠道文档
description: 这是创建第一个交互式通信文档的多步教程的最后一部分。 本部分，我们通过电子邮件了解Web渠道文档的投放。
seo-description: 这是创建第一个交互式通信文档的多步教程的最后一部分。 本部分，我们通过电子邮件了解Web渠道文档的投放。
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
translation-type: tm+mt
source-git-commit: 4f51f7bf00827210d2631b9335768a9980f6655c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# 建立Web投放渠道文档 {#setting-up-the-delivery-of-web-channel-document}


本部分，我们通过电子邮件了解Web渠道文档的投放。

定义并测试Web渠道交互式通信文档后，您需要一种投放机制将Web渠道文档交付给收件人。

为了能够将电子邮件用作Web投放文档的渠道机制，我们需要对表单数据模型做出细微的更改。

[通过电子邮件进一步了解Web渠道投放](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

登录AEM Forms。

* 导航到Forms->数据集成

* 在编辑模式下打开RetirementAccountStatement数据模型。

* 选择余额对象，然后单击编辑按钮。

* 选择“铅笔”图标以在编辑模式下打开id参数。

* 将绑定更改为“RequestAttribute”。

* 在绑定值中设置帐号，如下所示。

* 这样，我们会通过请求属性将帐户编号传递到表单数据模型

* 确保保存更改。
   ![fdm](assets/requestattribute.gif)

## 测试Web投放渠道的电子邮件文档 {#test-email-delivery-of-web-channel-document}

* [使用包管理器安装示例资产](assets/webchanneldelivery.zip)
* [登录crx](http://localhost:4502/crx/de/index.jsp#)

* 导航到/home/users

* 在用户节点下搜索管理员用户。

* 选择管理员用户的用户档案节点。

* 创建名为“accountnumber”的属性。 确保属性类型为字符串。

* 将此帐户编号属性的值设置为“3059827”。 您可以根据需要将此值设置为任意随机数。

* [打开getad.html](http://localhost:4502/content/getad.html)

* 与此URL关联的代码将获取已登录用户的帐号。 此帐户号随后作为request属性传递给FDM。 然后，FDM将获取与此帐户号关联的数据并填充Web渠道文档。
>[!NOTE]
请查看crx中的 **/apps/AEMForms/fetchad/GET.jsp** 文件。 请确保字符串变量webChannelDocument指向有效的通信文档路径。
