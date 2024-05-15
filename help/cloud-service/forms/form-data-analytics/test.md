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
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# 测试您的解决方案

使用多种表单值组合预览并提交表单。 请在Adobe Analytics报表中等待几到30分钟以查看您的数据。 设置为prop的数据比设置为eVar的数据更快地显示在报表中。

## 报表包

在Adobe Analytics中捕获的表单数据以圆环格式显示

**各国提交的材料**

![applicantsbystate](assets/donut.png)

字段验证错误

![field-validation-error](assets/donut-field-validation.png)

## 调试

确保自适应表单使用的配置容器与包含AdobeLaunch配置的容器相同。

要确认表单向Adobe Analytics发送数据，请执行以下操作

* 在浏览器中打开开发人员工具。
* 在控制台面板的以下文本中输入。

```javascript
_satellite.setDebug(true)
```

与表单交互，同时保持控制台窗口打开。 您应该看到类似这样的内容

![console-debug](assets/debug.png)

## 使用Adobe Experience Platform Debugger

添加 [AEP调试器扩展](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) ，则需要登录)以获取更多调试信息

![platform-debugger](assets/platform-debugger.png)

## 恭喜

您已成功将AEM Formsas a Cloud Service与Adobe Analytics集成以报告表单数据字段。