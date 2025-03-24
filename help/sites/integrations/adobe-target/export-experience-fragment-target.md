---
title: 将体验片段导出到Adobe Target
description: 了解如何将AEM Experience Fragment发布和导出为Adobe Target选件。
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service， AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 3%

---

# 导出Experience Fragment到Adobe Target {#experience-fragment-target}

了解如何将AEM Experience Fragment导出为Adobe Target产品。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用体验片段选件创建Target活动](./create-target-activity.md)

## 疑难解答

### 将体验片段导出到Target失败

#### 错误

如果将Experience Fragment导出到Adobe Target时没有Adobe Admin Console中的正确权限，则会导致AEM Author服务出现以下错误：

![Target API UI错误](assets/error-target-offer.png)

...和`aemerror`日志中的以下日志消息：

![Target API控制台错误](assets/target-console-error.png)

#### 解决方法

1. 登录至[Admin Console](https://adminconsole.adobe.com/)，并具有Adobe Target产品配置文件的管理权限，但使用的是AEM集成
2. 选择&#x200B;__产品> Adobe Target >产品配置文件__
3. 在&#x200B;__集成__&#x200B;选项卡下，为您的AEM as a Cloud Service环境选择集成(与Adobe Developer项目同名)
4. 分配&#x200B;__编辑者__&#x200B;或&#x200B;__审批者__&#x200B;角色

   ![目标API错误](assets/target-permissions.png)

将正确的权限添加到Adobe Target集成应可解决此错误。

## 支持链接

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
