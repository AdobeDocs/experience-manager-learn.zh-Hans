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
source-wordcount: '180'
ht-degree: 1%

---

# 测试您的解决方案

使用多个表单值组合预览和提交表单。 在Adobe Analytics报表中，需要多到30分钟才能查看您的数据。 数据集为prop的数据在报表中显示的时间早于数据集为eVar的数据。

## 报表包

在Adobe Analytics中捕获的表单数据以圆环形格式显示

**各国提交的材料**

![应用程序bystate](assets/donut.png)

字段验证错误

![字段验证 — 错误](assets/donut-field-validation.png)

## 调试

确保自适应表单使用的配置容器与包含Adobe启动配置的容器相同。

要确认表单正在向Adobe Analytics发送数据，请执行以下操作

* 在浏览器中打开开发人员工具。
* 在控制台面板的以下文本中输入。

```javascript
_satellite.setDebug(true)
```

在保持控制台窗口打开的同时与表单进行交互。 你应该看到这样的东西

![控制台调试](assets/debug.png)

## 使用Adobe Experience Platform Debugger

添加 [AEP调试器扩展](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) 浏览器（您需要登录）以获取更多调试信息

![platform-debugger](assets/platform-debugger.png)





