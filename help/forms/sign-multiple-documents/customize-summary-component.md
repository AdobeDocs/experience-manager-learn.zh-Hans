---
title: 自定义摘要组件
description: 扩展摘要步骤组件以包含导航到包中下一个表单的功能。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 2%

---


# 自定义摘要步骤

摘要步骤组件用于显示表单提交的摘要，其中包含下载已签名表单的链接。 摘要步骤通常放置在表单的最后一个面板中。
为此用例，我们基于开箱即用的“摘要”组件创建了一个新组件，并扩展了包含自定义clientlib的功能。

此组件由“签名多个表单”标签标识

以下屏幕快照显示了为在签署仪式结束时显示消息而创建的新组件

![摘要组件](assets/summary.PNG)

新组件基于开箱即用的摘要组件。
![component-prop](assets/componentprop.PNG)

我们添加了一个按钮，可导航到下一个用于签名的表单
![template-code](assets/template-code.PNG)

summary.jsp具有以下代码。 它引用了由类别id **getnextform**&#x200B;标识的客户端库

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## 资产

可从此处下载自定义摘要组件[](assets/custom-summary-step.zip)


