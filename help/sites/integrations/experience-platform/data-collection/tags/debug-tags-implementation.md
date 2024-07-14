---
title: 调试标记实施
description: 介绍调试Tags实施的一些常用工具和技术。 了解如何使用浏览器中的开发人员控制台和Experience Platform调试器扩展功能，识别Tags实施的关键方面并执行相应的故障诊断。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# 调试标记实施 {#debug-tags-implementation}

介绍用于调试Tags实施的常用工具和技术。 了解如何使用浏览器中的开发人员控制台和Experience Platform调试器扩展功能，识别Tags实施的关键方面并执行相应的故障诊断。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## 通过Satellite对象进行客户端调试

客户端调试有助于验证标记属性规则的加载或执行顺序。 每当向网站添加Tag属性时，浏览器中都会显示`_satellite` JavaScript对象，以促进客户端事件和数据跟踪。

要启用客户端调试，请在`_satellite`对象中调用`setDebug(true)`方法。

1. 打开浏览器控制台，然后运行以下命令。

   ```javascript
       _satellite.setDebug(true);
   ```

1. 重新加载AEM站点页面，并验证控制台日志是否显示如下所示的&#x200B;_触发的规则_&#x200B;消息。

   创作和Publish页面上的![标记属性](assets/satellite-object-debugging.png)

## 通过Adobe Experience Platform Debugger调试

Adobe提供Adobe Experience Platform Debugger[Chrome扩展](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)来调试、了解该集成并获取其见解。

1. 打开Adobe Experience Platform Debugger扩展，然后在Publish实例上打开站点页面

2. 在&#x200B;**Adobe Experience Platform Debugger>摘要> Adobe Experience Platform Tags**&#x200B;部分中，验证Tag属性的详细信息，如名称、版本、内部版本日期、环境和扩展。

   ![Adobe Experience Platform Debugger和标记属性详细信息](assets/tag-property-details.png)

## 其他资源 {#additional-resources}

+ [Adobe Experience Platform Debugger简介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Satellite对象引用](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
