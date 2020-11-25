---
title: 将体验片段导出至Adobe Target
description: 了解如何将AEM体验片段作为Adobe Target优惠发布和导出。
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---


# Export Experience Fragment to Adobe Target {#experience-fragment-target}

了解如何将AEM体验片段导出为Adobe Target优惠。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用体验片段目标创建活动优惠](./create-target-activity.md)

## 疑难解答

### 将体验片段导出到目标失败

#### 错误

将体验片段导出到Adobe Target时，在Adobe Admin Console没有正确的权限，将导致AEM作者服务出现以下错误：

    ![目标API UI错误](assets/error-target-offer.png)

...和日志中的以下日志 `aemerror` 消息：

    ![目标API控制台错误](assets/target-console-error.png)

#### 分辨率

1. 登录 [Admin Console](https://adminconsole.adobe.com/) ，具有Adobe Target产品用户档案的管理权限，但使用AEM集成
2. 选择 __产品>Adobe Target>产品用户档案__
3. 在“ __集成__ ”选项卡下，选择AEM的集成作为Cloud Service环境(与AdobeI/O项目同名)
4. 分配 __编辑__ 或审 __批人角色__

   ![目标API错误](assets/target-permissions.png)

向您的Adobe Target集成添加正确权限应解决此错误。

## 支持链接

+ [Adobe Experience Cloud调试器- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud调试器- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)