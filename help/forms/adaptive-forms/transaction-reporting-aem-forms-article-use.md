---
title: 在AEM Forms中使用Transaction Reporting
description: 利用AEM Forms中的交易报表，可计数自AEM Forms部署中的指定日期以来发生的所有交易。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 96
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# 在AEM Forms中使用Transaction Reporting{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1引入了事务报表功能，用于捕获表单提交次数、使用文档服务呈现文档以及呈现交互式通信（Web和打印渠道）。此功能主要适用于希望根据提交的表单和/或提交的文档数量来许可软件的客户。 此功能当前仅在AEM Forms OSGI栈栈上可用。

## 启用交易报告 {#enabling-transaction-reporting}

默认情况下，事务记录处于禁用状态。 要启用交易记录功能，请执行以下步骤：

* [打开configMgr](http://localhost:4502/system/console/configMgr)
* 搜索“Forms Transaction Reporting”
* 选中“记录交易记录”复选框
* 保存更改

启用事务报表后，您可以提交Adaptive Forms、使用文档服务生成文档或渲染Interactive Communication文档以查看事务报表的实际执行情况。

## 查看事务处理报表 {#viewing-transaction-report}

要查看事务报告，请以管理员身份登录AEM Forms。 只有fd-Administrator组的成员才能查看事务报告。

选择工具 |Forms |查看交易报告

或者通过单击查看事务报告 [此处](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

在上面的屏幕快照中，已处理文档是使用文档服务生成的文档数。 呈现的文档是呈现的交互式通信文档（Web和打印）的数量。 Forms提交的自适应表单提交次数。

事务在缓冲区中保留指定的时间段（刷新缓冲区时间+反向复制时间）。 默认情况下，事务计数大约需要90秒才能反映在事务报表中。

提交PDF表单、使用代理UI预览交互式通信或使用非标准表单提交方法等操作不计为交易。 AEM Forms提供了一个API来记录此类交易。 从自定义实施中调用API以记录交易。

如果您在创作实例上查看事务报告，请确保在所有发布实例上配置了反向复制。

要了解有关交易报告的更多信息，请执行以下操作 [请单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
