---
title: 创建Acrobat Sign云配置Cloud Service
description: 使用云服务配置创建AEM Forms和Acrobat Sign集成。
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

AEM中的云服务配置允许您在AEM与其他云应用程序之间创建集成。

以下视频将指导您完成创建云服务配置以将AEM与Acrobat Sign集成所需的步骤

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 疑难解答

如果配置Abobe Sign云克隆配置时出错，可以采取以下步骤进行故障诊断
* 确保在Acrobat Sign API应用程序中指定的重定向URL格式如下
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
例如： https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要保存云配置的容器的名称
* 确保oAuth URL正确无误
* 检查您的客户端ID和客户端密钥
* 尝试隐身窗口模式

