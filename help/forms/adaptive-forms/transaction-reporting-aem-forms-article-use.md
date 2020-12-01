---
title: 在AEM Forms使用交易报告
seo-title: 在AEM Forms使用交易报告
description: 在AEM Forms，事务报表允许您对自指定日期以来在AEM Forms部署时发生的所有事务进行计数。
seo-description: 在AEM Forms，事务报表允许您对自指定日期以来在AEM Forms部署时发生的所有事务进行计数。
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: adaptive-forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 1%

---


# 在AEM Forms使用事务报告{#using-transaction-reporting-in-aem-forms}

AEM Forms6.4.1引入了交易报告，以捕获表单提交数量、使用文档服务呈现文档以及交互通信(Web和打印渠道)呈现。此功能主要针对希望根据表单提交数量和／或呈现文档来许可软件的客户。 此功能目前仅在AEM FormsOSGI堆栈上可用。

## 启用事务报告{#enabling-transaction-reporting}

默认情况下，禁用事务记录。 要启用事务记录，请按照以下步骤操作：

* [打开configMgr](http://localhost:4502/system/console/configMgr)
* 搜索“Forms交易报告”
* 选中“记录事务”复选框
* 保存更改

启用事务报告后，您可以提交自适应Forms、使用文档服务生成文档或渲染交互通信文档，以查看实际操作中的事务报告。

## 查看事务报告{#viewing-transaction-report}

要视图事务报告，请以管理员身份登录AEM Forms。 只有fd-Administrator组的成员才能视图事务报表。

选择工具 |Forms |视图事务报表

或单击[此处](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)视图事务报告

![TransctionReporting](assets/transactionreporting.gif)

在上面的“已处理文档”屏幕截图中，是使用文档服务生成的文档数。 呈现的文档是呈现的交互式通信文档（Web和打印）的数量。 Forms提交的是自适应表单提交数。

在指定的时间段内，事务将保留在缓冲区中（刷新缓冲时间+反向复制时间）。 默认情况下，事务计数大约需要90秒才能反映在事务报表中。

诸如提交PDF表单、使用代理UI预览交互式通信或使用非标准表单提交方法等操作不作为事务处理入账。 AEM Forms提供API记录此类交易。 从您的自定义实现调用API以记录事务。

如果要查看创作实例上的事务报告，请确保在所有发布实例上都配置了反向复制。

要进一步了解事务报告[，请单击此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

