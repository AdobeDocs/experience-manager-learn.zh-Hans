---
title: 在AEM Forms中使用事务报告
seo-title: 在AEM Forms中使用事务报告
description: AEM Forms中的事务报表允许您对自指定日期以来在AEM Forms部署中发生的所有事务进行计数。
seo-description: AEM Forms中的事务报表允许您对自指定日期以来在AEM Forms部署中发生的所有事务进行计数。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: 自适应表单
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 2%

---


# 在AEM Forms{#using-transaction-reporting-in-aem-forms}中使用事务报告

AEM Forms 6.4.1中引入了交易报告，用于捕获表单提交数量、使用文档服务渲染文档和交互通信(Web和打印渠道)渲染。此功能主要针对希望根据表单提交数量和/或渲染数量来许可软件的客户。 此功能当前仅在AEM Forms OSGI堆栈上可用。

## 启用事务报告{#enabling-transaction-reporting}

默认情况下，会禁用事务记录。 要启用事务记录，请按照以下步骤操作：

* [打开configMgr](http://localhost:4502/system/console/configMgr)
* 搜索“Forms事务报告”
* 选中“记录事务”复选框
* 保存更改

启用事务报告后，您可以提交自适应Forms，使用文档服务生成文档，或渲染Interactive Communication文档，以查看事务报告的实际操作。

## 查看事务报表{#viewing-transaction-report}

要视图事务报表，请以管理员身份登录AEM Forms。 只有fd-Administrator组的成员才能视图事务报表。

选择工具 | Forms |视图事务报表

或单击[此处](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)视图事务报表

![TransctionReporting](assets/transactionreporting.gif)

在以上“已处理文档”屏幕截图中，是使用文档服务生成的文档数。 渲染的文档是渲染的交互式通信文档（Web和打印）的数量。 Forms Submitted是自适应表单提交的数量。

事务在指定时间段内仍保留在缓冲区中（刷新缓冲区时间+反向复制时间）。 默认情况下，事务计数大约需要90秒才能反映在事务报表中。

诸如提交PDF表单、使用代理UI预览交互式通信或使用非标准表单提交方法等操作不作为事务处理入账。 AEM Forms提供了用于记录此类事务的API。 从您的自定义实施调用API以记录事务。

如果您正在查看创作实例上的事务报表，请确保在所有发布实例上都配置了反向复制。

要了解有关事务报告[的更多信息，请单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

