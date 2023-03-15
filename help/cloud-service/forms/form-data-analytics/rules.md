---
title: 使用Adobe Analytics报告已提交的表单数据字段
description: 将AEM Forms CS与Adobe Analytics集成以报告表单数据字段
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 定义规则

在标记属性中，我们新建了2个 [规则](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**字段验证错误和表单提交**)。

![自适应表单](assets/rules.png)


## 字段验证错误

的 **字段验证错误** 每当自适应表单字段中出现验证错误时，都会触发规则。 例如，在我们的表单中，如果电话号码或电子邮件的格式不符合预期，则会显示验证错误消息。

通过将事件设置为 _**Adobe Experience Manager Forms错误**_ 如屏幕快照中所示



![申请人 — 国家居住地](assets/field_validation_error_rule.png)

Adobe Analytics — 设置变量的配置如下所示

![设置操作](assets/field_validation_action_rule.png)

## 表单提交规则

每次成功提交自适应表单时，都会触发表单提交规则。

表单提交规则使用 _**Adobe Experience Manager Forms — 提交**_ 事件

![form-submit-rule](assets/form-submit-rule.png)

在表单提交规则中，数据元素的值 _**ApplicentsStateOfResidence**_ 映射到prop5，数据元素FormTitle的值映射到prop8。

Adobe Analytics — 设置变量的配置如下所示。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

准备好测试标记代码后，[发布对标记所做的更改](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 使用发布流程。
