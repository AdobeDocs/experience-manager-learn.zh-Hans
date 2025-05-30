---
title: 使用AEM中的系统概述功能板
description: 在AEM的早期版本中，管理员需要查看多个位置，才能全面了解AEM实例。 “系统概述”旨在通过从单个仪表板提供AEM实例的配置、硬件和运行状况的高级别视图，来解决此问题。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Operations
doc-type: Technical Video
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
duration: 357
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 使用系统概述功能板

Adobe Experience Manager (AEM) [!UICONTROL 系统概述]提供了从单个仪表板全面了解AEM实例的配置、硬件和运行状况的视图。

>[!VIDEO](https://video.tv.adobe.com/v/40160?quality=12&learn=on&captions=chi_hans)

1. 系统概述可从以下位置访问： **AEM开始** > **[!UICONTROL 工具]** > **[!UICONTROL 操作]** > **[!UICONTROL 系统概述]**

   直接在&#x200B;**`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 单击[!UICONTROL 下载]按钮可以导出[!UICONTROL 系统概述]中的信息。 该信息也通过以下[!DNL REST]端点公开：
1. 以下是从[!UICONTROL 系统概述]导出的JSON输出示例：

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
