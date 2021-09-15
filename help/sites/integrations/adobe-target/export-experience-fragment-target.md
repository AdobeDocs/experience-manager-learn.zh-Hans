---
title: 将体验片段导出到Adobe Target
description: 了解如何将AEM Experience Fragment作为Adobe Target选件发布和导出。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 2%

---

# 将体验片段导出到Adobe Target {#experience-fragment-target}

了解如何将AEM Experience Fragment导出为Adobe Target选件。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用体验片段选件创建Target活动](./create-target-activity.md)

## 疑难解答

### 将体验片段导出到Target失败

#### 错误

在Adobe Admin Console中将体验片段导出到Adobe Target时，如果没有正确的权限，则会在AEM创作服务中导致以下错误：

    ![Target API UI错误](assets/error-target-offer.png)

...和`aemerror`日志中的以下日志消息：

    ![Target API控制台错误](assets/target-console-error.png)

#### 解决方法

1. 登录到[Admin Console](https://adminconsole.adobe.com/)，其中包含所用Adobe Target产品配置文件(但是AEM集成)的管理权限
2. 选择&#x200B;__产品>Adobe Target >产品配置文件__
3. 在&#x200B;__集成__&#x200B;选项卡下，选择AEM的集成作为Cloud Service环境(与Adobe I/O项目同名)
4. 分配&#x200B;__编辑者__&#x200B;或&#x200B;__审批者__&#x200B;角色

   ![Target API错误](assets/target-permissions.png)

向Adobe Target集成添加正确权限应会解决此错误。

## 支持链接

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
