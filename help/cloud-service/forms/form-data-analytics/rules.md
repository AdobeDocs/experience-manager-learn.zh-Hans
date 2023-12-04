---
title: 使用Adobe Analytics报告已提交的表单数据字段
description: 将AEM Forms CS与Adobe Analytics集成以报告表单数据字段
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 62
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# 定义规则

在Tags属性中，我们创建了2个新的 [规则](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**字段验证错误和表单提交**)。

![自适应表单](assets/rules.png)


## 字段验证错误

此 **字段验证错误** 每当自适应表单字段中存在验证错误时，就会触发规则。 例如，在我们的表单中，如果电话号码或电子邮件不是预期格式，则会显示验证错误消息。

字段验证错误规则是通过将事件设置为来配置的 _**Adobe Experience Manager Forms-Error**_ 如屏幕快照中所示



![申请人 — 国家 — 居所](assets/field_validation_error_rule.png)

Adobe Analytics — 设置变量的配置如下

![设置操作](assets/field_validation_action_rule.png)

## 表单提交规则

每次成功提交自适应表单时都会触发表单提交规则。

使用以下方式配置表单提交规则 _**Adobe Experience Manager Forms — 提交**_ 事件

![form-submit-rule](assets/form-submit-rule.png)

在表单提交规则中，数据元素的值 _**ApplicationsStateOfResidence**_ 映射到prop5，并且数据元素FormTitle的值映射到prop8。

Adobe Analytics - Set变量的配置如下。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

当您准备好测试标记代码时，[发布对标记所做的更改](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 使用发布流。

## 后续步骤

[测试解决方案](./test.md)