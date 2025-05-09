---
title: 创建Acrobat Sign云配置Cloud Service
description: 使用AEM Forms服务配置创建Cloud Services与Acrobat Sign集成。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 222
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# 创建Acrobat Sign云配置

AEM中的Cloud Services配置允许您在AEM和其他云应用程序之间创建集成。

以下视频将指导您完成创建AEM与Acrobat Sign集成的云服务配置所需的步骤

>[!VIDEO](https://video.tv.adobe.com/v/3411766?quality=12&learn=on&captions=chi_hans)

## 疑难解答

如果您在配置Abobe Sign云配置时遇到错误，可以采取以下步骤进行故障诊断
* 确保Acrobat Sign API应用程序中指定的重定向URL的格式如下
&lt;您的实例名称>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;容器>。
例如 — https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要包含云配置的容器的名称
* 确保oAuth URL正确
* 检查您的客户端ID和客户端密码
* 尝试无痕窗口模式

