---
title: 将体验片段导出到Adobe Target
description: 了解如何将AEM Experience Fragment发布和导出为Adobe Target选件。
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: e9c0974d35493a607969124b2906564fc97bcdea
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 4%

---

# 导出Experience Fragment到Adobe Target {#experience-fragment-target}

了解如何将AEM Experience Fragment导出为Adobe Target优惠。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 后续步骤

+ [使用体验片段选件创建Target活动](./create-target-activity.md)

## 疑难解答

### 将体验片段导出到Target失败

#### 错误

如果将Experience Fragment导出到Adobe Target时没有Adobe Admin Console中的正确权限，则会导致AEM Author服务出现以下错误：

![Target API UI错误](assets/error-target-offer.png)

...以及中的以下日志消息 `aemerror` 日志：

![Target API控制台错误](assets/target-console-error.png)

#### 解决方法

1. 登录 [Admin Console](https://adminconsole.adobe.com/) 已使用Adobe Target产品配置文件的管理权限，但AEM集成
2. 选择 __产品> Adobe Target >产品配置文件__
3. 下 __集成__ 选项卡，为您的AEMas a Cloud Service环境选择集成(与Adobe Developer项目同名)
4. 分配 __编辑者__ 或 __审批者__ 角色

   ![Target API错误](assets/target-permissions.png)

将正确的权限添加到Adobe Target集成应可解决此错误。

## 支持链接

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
