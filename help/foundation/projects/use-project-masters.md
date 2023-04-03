---
title: 如何在AEM中使用项目主页
description: 使用AEM Projects，项目主管可以大大简化用户和团队管理。
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# 使用项目主页

项目管理员可通过 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理员现在可以创建 **[!DNL Master Project]** 并作为项目团队的一部分，将用户分配给角色/权限。 可以从主控项目创建项目，并自动继承团队成员资格。 这具有以下几个优势：

* 在多个项目中重复使用现有团队
* 加快了项目的创建速度，因为无需手动重新创建团队
* 项目会自动从中心位置管理团队成员资格，并对团队进行的任何更新
* 避免创建可能导致性能问题的重复ACL

[!DNL Master Projects] 可在下创建 [!UICONTROL 大师] 文件夹 [!UICONTROL AEM项目]. 创建主控项目后，该项目会作为一个选项显示在创建新项目时向导中可用模板旁边。

[!DNL Project Masters] URL（本地AEM创作实例）： [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 删除 [!DNL Project Masters]

删除主控项目会导致派生项目不可用。

在删除主控项目之前，请确保完成所有派生项目并从AEM中删除。 在删除派生项目之前，请确保保存所有必需的项目数据。 从AEM中删除所有派生项目后，可以安全地删除主控项目。

## 标记 [!DNL Project Masters] 不活动

通过将主控项目的状态更改为项目属性中的不活动状态，不活动的主控项目会从主控项目列表中消失。

要显示不活动的主控项目，请切换顶部栏中的“显示活动”过滤器按钮（位于列表显示切换开关旁边）。 要使不活动的项目再次处于活动状态，只需选择不活动的主控项目，编辑项目属性，然后再次将其设置为活动。

## 了解 [!DNL Project Masters]

![项目主技术视图](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 通过使用定义一组AEM用户组（所有者、编辑者和观察者）并允许派生项目引用和重复使用这些集中定义的用户组来进行工作。

这会减少AEM中所需的用户组总数。 之前 [!DNL Project Masters]，每个项目都创建了3个用户组以及随附的ACE以强制执行权限授予，因此，100个项目产生了300个用户组。 项目主体允许任意数量的项目重复使用相同的三个组，前提是共享成员资格符合项目中的业务要求。
