---
title: 创建Acrobat Sign云配置Cloud Service
description: 使用Cloud Services配置创建AEM Forms和Acrobat Sign集成。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

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
* 检查您的客户端Id和客户端密钥
* 尝试无痕窗口模式

