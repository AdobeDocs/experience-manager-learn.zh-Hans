---
title: 创建Acrobat Sign云配置Cloud Service
description: 使用AEM Forms服务配置创建Cloud Services与Acrobat Sign集成。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 225
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# 创建Acrobat Sign云配置

AEM中的云服务配置允许您创建AEM与其他云应用程序之间的集成。

以下视频将指导您完成创建AEM与Acrobat Sign集成的云服务配置所需的步骤

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 疑难解答

如果您在配置Abobe Sign云配置时遇到错误，可以采取以下步骤进行故障诊断
* 确保Acrobat Sign API应用程序中指定的重定向URL的格式如下
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
例如 — https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要包含云配置的容器的名称
* 确保oAuth URL正确
* 检查您的客户端ID和客户端密码
* 尝试无痕窗口模式

