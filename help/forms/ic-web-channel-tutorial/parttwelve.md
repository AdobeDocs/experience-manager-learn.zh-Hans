---
title: 设置Web渠道文档的提交
seo-title: 设置Web渠道文档的提交
description: 这是创建首个交互式通信文档的多步教程的最后一个部分。 在本部分中，我们将介绍如何通过电子邮件交付Web渠道文档。
seo-description: 这是创建首个交互式通信文档的多步教程的最后一个部分。 在本部分中，我们将介绍如何通过电子邮件交付Web渠道文档。
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: 交互式通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---


# 设置Web渠道文档的传送{#setting-up-the-delivery-of-web-channel-document}


在本部分中，我们将介绍如何通过电子邮件交付Web渠道文档。

定义并测试Web渠道交互式通信文档后，您需要一种传送机制来将Web渠道文档传送给收件人。

为了能够将电子邮件用作Web渠道文档的投放机制，我们需要对表单数据模型进行细微更改。

[了解有关通过电子邮件投放Web渠道的更多信息](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

登录AEM Forms。

* 导航到Forms ->数据集成

* 在编辑模式下打开ReriementAccountStatement数据模型。

* 选择余额对象并单击编辑按钮。

* 选择“铅笔”图标以在编辑模式下打开id参数。

* 将绑定更改为“RequestAttribute”。

* 在绑定值中设置帐号，如下所示。

* 这样，我们就会通过表单数据模型的请求属性传入accountnumber

* 确保保存更改。
   ![fdm](assets/requestattribute.gif)

## 测试Web渠道文档的电子邮件投放{#test-email-delivery-of-web-channel-document}

* [使用包管理器安装示例资产](assets/webchanneldelivery.zip)
* [登录到crx](http://localhost:4502/crx/de/index.jsp#)

* 导航到/home/users

* 在用户的节点下搜索管理员用户。

* 选择管理员用户的配置文件节点。

* 创建名为“accountnumber”的属性。 确保属性类型是字符串。

* 将此accountnumber属性的值设置为“3059827”。 您可以根据需要将此值设置为任意随机数。

* [打开getad.html](http://localhost:4502/content/getad.html)

* 与此URL关联的代码将获取已登录用户的帐号。 然后，此帐号将作为请求属性传递到FDM。 然后，FDM将获取与此帐号关联的数据并填充Web渠道文档。

>[!NOTE]
>
>请查看crx中的&#x200B;**/apps/AEMForms/fetchad/GET.jsp**&#x200B;文件。 请确保字符串变量webChannelDocument指向有效的通信文档路径。
