---
title: 自定义摘要组件
description: 扩展摘要步骤组件，使其包含导航到资源包中下一个表单的功能。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 38
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# 自定义摘要步骤

摘要步骤组件用于显示表单提交的摘要，其中包含下载已签名表单的链接。 摘要步骤通常放置在表单的最后一个面板中。
出于此用例的目的，我们基于开箱即用的摘要组件创建了一个新组件，并扩展了包含自定义clientlib的功能。

此组件由“为多个表单签名”标签标识

以下屏幕抓图显示了创建的新组件，用于在签署仪式完成时显示消息

![摘要组件](assets/summary.PNG)

新组件基于开箱即用的摘要组件。
![component-prop](assets/componentprop.PNG)

我们已添加按钮以导航到下一个要签名的表单
![template-code](assets/template-code.PNG)

summary.jsp具有以下代码。 它引用了类别ID **getnextform**&#x200B;标识的客户端库

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## 资源

自定义摘要组件可以[从此处](assets/custom-summary-step.zip)下载

## 后续步骤

[获取下一个要签名的表单](./create-client-lib.md)