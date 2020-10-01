---
title: 创建Adobe TargetCloud Service帐户
description: 分步演练如何使用Cloud Service和AdobeIMS认证将Adobe Experience Manager作为Cloud Service与Adobe Target整合
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# 创建Adobe TargetCloud Service帐户 {#adobe-target-cloud-service}

逐步演练如何使用Cloud Service和AdobeIMS认证将Adobe Experience Manager作为Cloud Service与Adobe Target整合。

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>视频中显示的Adobe TargetCloud Services配置存在已知问题。 建议改为执行视频中的相同步骤，但使用旧版Adobe Target [Cloud Services配置](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)。

## 常见问题

在将体验片段导出到Adobe目标时，如果您在管理控制台中的目标集成没有正确的权限，您可能会收到错误，如下所示。

**UI错误**![目标API UI错误](assets/error-target-offer.png)

**日志错误**![目标API控制台错误](assets/target-console-error.png)


**解决方案**

1. 导航到 [Admin Console](https://adminconsole.adobe.com/)
2. 选择产品>Adobe Target>产品用户档案
3. 在“集成”选项卡下，选择您的集成(AdobeI/O项目)
4. 分配编辑者或审批者角色。

![目标API错误](assets/target-permissions.png)

为目标集成添加权限应有助于您解决上述问题。