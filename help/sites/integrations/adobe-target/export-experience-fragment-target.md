---
title: 将体验片段导出到Adobe Target
description: 了解如何将AEM Experience Fragment发布并导出为Adobe Target优惠。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 3%

---


# 将体验片段导出到Adobe Target {#experience-fragment-target}

了解如何将AEM体验片段导出为Adobe Target优惠。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用体验片段目标创建活动优惠](./create-target-activity.md)

## 疑难解答

### 将体验片段导出到目标失败

#### 错误

在Adobe Admin Console中将体验片段导出到Adobe Target时，如果没有正确的权限，则会在AEM作者服务上导致以下错误：

    ![目标 API UI错误](assets/error-target-offer.png)

...和`aemerror`日志中的以下日志消息：

    ![目标 API控制台错误](assets/target-console-error.png)

#### 分辨率

1. 登录[Admin Console](https://adminconsole.adobe.com/)，但使用AEM集成的Adobe Target产品用户档案具有管理权限
2. 选择&#x200B;__产品> Adobe Target >产品用户档案__
3. 在&#x200B;__集成__&#x200B;选项卡下，选择AEM的集成作为Cloud Service环境(与Adobe I/O项目同名)
4. 分配&#x200B;__编辑者__&#x200B;或&#x200B;__审批者__&#x200B;角色

   ![目标API错误](assets/target-permissions.png)

向Adobe Target集成添加正确权限应可解决此错误。

## 支持链接

+ [Adobe Experience Cloud调试器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud调试器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)