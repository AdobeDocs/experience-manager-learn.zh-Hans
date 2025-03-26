---
title: 在HTML5表单提交时触发AEM工作流程
description: 在提交移动表单时触发AEM工作流
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# 在移动表单提交时触发AEM工作流

一个常见用例是将XDP渲染为HTML以进行数据捕获活动。 在提交此表单时，可能需要触发AEM工作流程。 在AEM工作流中，您可以将数据与XDP模板合并，并提交生成的PDF以供审阅和批准。 表单在发布实例上呈现，工作流在AEM处理实例上触发。

用例涉及以下步骤

* 用户填写并提交HTML5表单(XDP的HTML5演绎版)。
* 提交由发布实例中的servlet处理。
* 此servlet将数据存储在AEM处理实例的存储库中的预定义文件夹中。
* 工作流启动器配置为每次在特定文件夹下添加新文件时触发AEM工作流。

本教程将介绍完成上述用例所需的步骤。 [此处提供了与此教程相关的示例代码和资源。](./deploy-assets.md)


## 后续步骤

[处理表单提交](./handle-form-submission.md)
