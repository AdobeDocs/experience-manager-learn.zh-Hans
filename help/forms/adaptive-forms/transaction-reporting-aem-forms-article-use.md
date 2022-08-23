---
title: 在AEM Forms中使用交易报表
description: 利用AEM Forms中的事务报表，可统计自AEM Forms部署中指定日期以来发生的所有事务。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---

# 在AEM Forms中使用交易报表{#using-transaction-reporting-in-aem-forms}

AEM Forms 6.4.1引入了交易报告功能，用于捕获表单提交数量、使用文档服务渲染文档以及渲染交互式通信（Web和打印渠道）。此功能主要适用于希望根据表单提交数量和/或文档渲染数量授权软件的客户。 此功能当前仅在AEM Forms OSGi堆栈上可用。

## 启用交易报告 {#enabling-transaction-reporting}

默认情况下，事务记录处于禁用状态。 要启用交易记录，请按照以下步骤操作：

* [打开configMgr](http://localhost:4502/system/console/configMgr)
* 搜索“Forms Transaction Reporting”
* 选中“记录交易”复选框
* 保存更改

启用事务报表后，您可以提交自适应Forms、使用文档服务生成文档或渲染Interactive Communication文档，以查看事务报表的实际操作情况。

## 查看交易报表 {#viewing-transaction-report}

要查看事务报表，请以管理员身份登录AEM Forms。 只有fd-Administrator组的成员才能查看事务报表。

选择工具 | Forms |查看交易报表

或通过单击 [此处](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransactionReporting](assets/transactionreporting.gif)

在上面的屏幕截图中， Document Processed是使用文档服务生成的文档数。 呈现的文档是呈现的交互式通信文档（Web和打印）的数量。 Forms Submitted是自适应表单提交的数量。

事务在指定时段（刷新缓冲时间+反向复制时间）内保留在缓冲区中。 默认情况下，交易计数大约需要90秒才能反映在交易报表中。

诸如提交PDF表单、使用代理UI预览交互式通信或使用非标准表单提交方法之类的操作不会计为交易。 AEM Forms提供了用于记录此类交易的API。 从自定义实施中调用API以记录交易。

如果要查看创作实例上的事务报表，请确保在所有发布实例上配置了反向复制。

了解有关交易报告的更多信息 [请单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
