---
title: 如何在AEM中使用项目母版
description: 通过AEM项目，项目母版极大地简化了用户和团队管理。
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

# 使用项目母版

项目母版通过以下方式极大地简化了用户和团队管理 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理员现在可以创建 **[!DNL Master Project]** 并将用户作为项目团队的一部分分配给角色/权限。 项目可从主控的项目创建，并自动继承团队成员资格。 这具备以下几项优势：

* 在多个项目中重用现有团队
* 由于无需手动重新创建团队，因此可加快项目创建速度
* 从一个中心位置管理团队成员资格，项目会自动继承对团队的任何更新
* 避免创建可能导致性能问题的重复ACL

[!DNL Master Projects] 可以在下创建 [!UICONTROL 母版] 文件夹在 [!UICONTROL AEM项目]. 创建主控项目后，当创建新项目时，它会显示为向导中可用模板旁边的选项。

[!DNL Project Masters] URL（本地AEM创作实例）： [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 删除 [!DNL Project Masters]

删除主控的项目会导致派生的项目不可用。

在删除主控项目之前，请确保已完成所有派生项目并将其从AEM中删除。 确保在删除派生项目之前保存任何所需的项目数据。 一旦从AEM中删除了所有派生项目，就可以安全地删除主控的项目。

## 标记 [!DNL Project Masters] 作为非活动

通过在项目的属性中将主控项目的状态更改为非活动，非活动的主控项目会从主控项目列表中消失。

要显示不活动的主控项目，请切换顶部栏中的“显示活动”筛选器按钮（在列表显示切换旁边）。 要使非活动项目再次处于活动状态，只需选择非活动主控项目，编辑项目属性，然后再次将其设置为活动状态。

## 了解 [!DNL Project Masters]

![项目母版技术视图](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 通过定义一组AEM用户组（所有者、编辑者和观察者）并允许派生的项目引用和重用这些集中定义的用户组来工作。

这会减少AEM中所需的用户组总数。 早于 [!DNL Project Masters]，每个项目创建了3个用户组并随附了ACE以强制授予权限，因此100个项目生成了300个用户组。 项目母版允许任意数量的项目重复使用相同的三个组，前提是共享成员资格符合项目中的业务要求。
