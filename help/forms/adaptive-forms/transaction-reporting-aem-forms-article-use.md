---
title: 在AEM Forms中使用Transaction Reporting
description: 利用AEM Forms中的交易报告，可计数自AEM Forms部署上的指定日期以来发生的所有交易。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---

# 在AEM Forms中使用Transaction Reporting{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1引入了交易报告功能，用于捕获提交的表单数量、使用文档服务呈现文档以及呈现交互式通信（Web和打印渠道）。此功能主要适用于希望根据提交的表单和/或提交的文档数量来许可软件的客户。 此功能当前仅在AEM Forms OSGI栈栈上可用。

## 启用交易报告 {#enabling-transaction-reporting}

默认情况下，事务记录处于禁用状态。 要启用交易记录功能，请按照以下步骤操作：

* [打开configmgr](http://localhost:4502/system/console/configMgr)
* 搜索“Forms交易报告”
* 选中“记录交易”复选框
* 保存更改

启用交易报告后，您可以提交Adaptive Forms、使用文档服务生成文档或渲染Interactive Communication文档以查看交易报告的运行情况。

## 查看事务处理报表 {#viewing-transaction-report}

要查看事务报告，请以管理员身份登录AEM Forms。 只有fd-Administrator组的成员可以查看事务报告。

选择工具 |Forms |查看交易报告

或者通过单击查看事务处理报表 [此处](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

在上面的屏幕快照中， Document Processed是使用文档服务生成的文档数。 渲染的文档是渲染的交互式通信文档（Web和打印）的数量。 Forms Submitted是自适应表单提交次数。

事务在缓冲区中保留指定的时间段（刷新缓冲区时间+反向复制时间）。 默认情况下，事务计数大约需要90秒才能反映在事务报告中。

提交PDF表单、使用代理UI预览交互式通信或使用非标准表单提交方法等操作不会计为交易。 AEM Forms提供了一个API来记录此类交易。 从自定义实施中调用API以记录交易。

如果您正在查看创作实例上的事务报告，请确保在所有发布实例上配置了反向复制。

要了解有关交易报告的更多信息，请执行以下操作 [请单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
