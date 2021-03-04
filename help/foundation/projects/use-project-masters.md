---
title: 如何在AEM中使用Project Masters
description: Project Masters通过AEM Projects大大简化了用户和团队管理。
version: 6.4, 6.5, cloud-service
topic: 内容管理
feature: 项目
level: 中间
role: 业务从业者
kt: 256
thumbnail: 17740.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---


# 使用项目主页

Project Masters使用[!DNL AEM Projects]大大简化了用户和团队管理。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

管理员现在可以创建&#x200B;**[!DNL Master Project]**，并将用户分配给项目团队中的角色/权限。 可以从主控项目创建项目并自动继承团队成员资格。 这优惠了几个优势：

* 在多个项目中重复使用现有团队
* 加快项目创建，因为团队无需手动重新创建
* 从中心位置管理团队成员资格，项目会自动继承对团队的任何更新
* 避免创建可能导致性能问题的重复ACL

[!DNL Master Projects] 可以在AEM项目下的  Mastersfolder [!UICONTROL 下创建]。创建主控项目后，在新建项目时，该项目会在向导中的可用模板旁边显示为一个选项。

[!DNL Project Masters] URL（本地AEM作者实例）： [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 删除 [!DNL Project Masters]

删除主控项目会导致派生项目不可用。

在删除主控项目之前，请确保完成所有派生项目并从AEM中删除。 在删除派生的项目之前，请确保保存任何所需的项目数据。 从AEM中删除所有派生项目后，可安全删除主控项目。

## 将[!DNL Project Masters]标记为非活动

通过将主控项目的状态更改为项目属性中的非活动状态，不活动的主控项目将从主控项目列表中消失。

要显示不活动的主控项目，请切换顶栏中的“显示活动”筛选器按钮(列表显示切换的旁边)。 要使不活动的项目再次处于活动状态，只需选择不活动的主控项目，编辑项目属性，然后再次将其设置为活动状态。

## 了解[!DNL Project Masters]

![项目主管技术视图](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 通过定义一组AEM用户组（所有者、编辑者和观察者）并允许派生项目引用和重用这些集中定义的用户组，来工作。

这会减少AEM中所需的用户组总数。 在[!DNL Project Masters]之前，每个项目创建了3个用户组，其中随附的ACE用于强制执行权限，因此100个项目产生了300个用户组。 Project Master允许任意数量的项目重复使用相同的三个组，前提是共享会员资格符合项目中的业务要求。
