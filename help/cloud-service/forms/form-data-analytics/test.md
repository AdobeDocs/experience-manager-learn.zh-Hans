---
title: 使用Adobe Analytics报告提交的表单数据字段
description: 将AEM Forms CS与Adobe Analytics集成以报告表单数据字段
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 4100061624bd8955bee392f1eced20f388f2902c
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# 测试您的解决方案

使用多种表单值组合预览和提交表单。 请在Adobe Analytics报表中留出数到30分钟的时间查看您的数据。 设置为prop的数据比设置为eVar的数据更快地显示在报表中。

## 报表包

在Adobe Analytics中捕获的表单数据以圆环格式呈现。按州/省/自治区/直辖市

![applicantsbystate](assets/donut.png)

字段验证错误

![field-validation-error](assets/donut-field-validation.png)

## 调试

确保自适应表单使用的配置容器与包含Adobe启动配置的配置容器相同。

要确认表单将数据发送到Adobe Analytics，请执行以下操作

* 在浏览器中打开开发人员工具。
* 在Console面板的以下文本中输入。

```javascript
_satellite.setDebug(true)
```

与表单交互，同时保持控制台窗口打开。 你应该看到这样的东西

![console-debug](assets/debug.png)

## 使用Adobe Experience Platform调试器

添加 [AEP调试器扩展](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) （需要登录）以获取更多调试信息

![platform-debugger](assets/platform-debugger.png)





